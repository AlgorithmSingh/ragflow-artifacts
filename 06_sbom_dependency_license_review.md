# RAGFlow SBOM, Dependency, License, and Supply-Chain Review

## Purpose

RAGFlow has a large dependency surface across Python, Node.js, Go, Docker images, and externally downloaded binaries. This artifact defines the minimum SBOM and license review needed before approval.

## Top-level license

- RAGFlow repository license: **Apache License 2.0**.
- Third-party dependencies still require separate license and vulnerability review.

## Dependency sources observed

| Ecosystem | Manifest / source | Notes |
|---|---|---|
| Python | `pyproject.toml`, `uv.lock` | Large dependency set including LLM SDKs, connectors, parsing/OCR, browser/crawler, Langfuse, LiteLLM, Tika, Selenium-related packages. |
| Node/frontend | `web/package.json`, `web/package-lock.json` | React/Vite frontend and many UI/document-rendering dependencies. |
| Go | `go.mod`, `go.sum` | Go server/CLI/agent components, OpenTelemetry, Infinity SDK, internal services. |
| Docker images | `Dockerfile`, `docker/docker-compose*.yml`, `helm/values.yaml` | RAGFlow, Elasticsearch/OpenSearch, MySQL, MinIO, Valkey, Infinity, OceanBase/SeekDB, NATS, TEI, sandbox images. |
| External binary/script downloads | `Dockerfile`, `download_deps.py`, `build.sh`, `docker/launch_backend_service.sh` | Chrome, chromedriver, Tika JAR, uv, libssl debs, Nginx packages, NodeSource setup, Microsoft packages, GitHub/Gitee resource clone. |
| Model/provider config | `conf/llm_factories.json`, `conf/models/*.json` | Many external provider endpoints; not all should be approved. |

## Known supply-chain concerns to resolve

| Concern | Why it matters | Required mitigation |
|---|---|---|
| Dockerfile uses `infiniflow/ragflow_deps:latest` as a build source (6 bind-mount uses) | Mutable image can change without review | Pin by digest and mirror internally. |
| Compose image tags — mix of pinned and mutable | MinIO is pinned to a dated tag (`pgsty/minio:RELEASE.2026-03-25T00-00-00Z`); ES/MySQL/Valkey/etc. use fixed minor tags; **sandbox images use `:latest`** | Pin all images by digest; the genuinely mutable ones are the sandbox `:latest` images and `ragflow_deps:latest`. |
| `pyproject.toml` git dependency from Gitee (`graspologic`) | Git dependency bypasses normal package registry review | **Already commit-pinned** to `38e680cab72bc9fb68a7992c3bcc2d53b24e42fd` (`pyproject.toml:55`, `uv.lock`). Residual gap is mirroring + license/source review, not pinning. Note `Dockerfile:167` sed-swaps `gitee.com`→`github.com` at build time (same commit). |
| `uv.lock` resolved against the Tsinghua mirror | ~3288 URLs point at `pypi.tuna.tsinghua.edu.cn` vs 2 at `pypi.org`; `Dockerfile:162-167` sed-rewrites the host at build time (hashes preserved) | Re-resolve the lock against an approved internal mirror; confirm host-swap-with-locked-hash is acceptable to policy. |
| `office_oxide` native lib from `github.com/yfedoseev` (`build.sh:168`, v0.1.2) | Third-party GitHub account, **no checksum**, linked into Go binaries via CGO | Vendor, pin by digest, verify the prebuilt binary. |
| `en_core_web_sm` 3.8.0 wheel from `github.com/explosion` (`pyproject.toml:112`) | GitHub-direct wheel bypasses PyPI registry review | Hash-pinned in `uv.lock` (good); mirror internally. |
| Dockerfile/scripts download from GitHub/Gitee, Nginx, NodeSource, Microsoft, Ubuntu/Aliyun/Tsinghua mirrors, Chrome sources, Maven, OpenAI blob | Build supply chain spans many hosts; `goproxy.cn` is primary Go proxy | Use internal mirrors and verify checksums. **NodeSource is `curl | bash -` as root (`Dockerfile:92`); Microsoft uses deprecated `apt-key add` (`Dockerfile:103`)** — scope keys / pre-bake. |
| Large parser/browser stack | Exposed to untrusted uploaded files | Patch aggressively; scan CVEs; isolate ingestion. |
| `SECURITY.md` ships a working PoC for a live, unfixed RCE | Unsafe pickle: `restricted_loads` (`api/utils/configs.py:22-39`) whitelists `numpy`, reaching `numpy.f2py.diagnose.run_command`. The linked path `api/utils/__init__.py#L215` is **stale** (function moved to `configs.py`). Commit `547b8cf9d` removed the insecure-by-default `pickle.loads` fallback but explicitly left the `numpy` allowlist vulnerable. | Latent today (no in-tree `SerializedField(PICKLE)` column) but exploitable if attacker controls pickled bytes in a PICKLE column. Run image ≥ `547b8cf9d`; tighten `find_class` to an explicit symbol allowlist; block `numpy.f2py`; flip the `SerializedField` default to JSON. Tracked as R21 in `04`. |

## Required SBOM outputs

Generate and store these artifacts from the exact build being proposed:

- Container image SBOM for each runtime image.
- Python dependency SBOM from `uv.lock` / installed environment.
- Node dependency SBOM from `web/package-lock.json`.
- Go module SBOM from `go.mod` / built binaries.
- License report for all direct and transitive dependencies.
- Vulnerability scan report with triage notes.

## Suggested commands

Run from a controlled CI environment with approved scanners. Adjust image names/tags for the exact build.

```bash
# Container SBOM
syft infiniflow/ragflow:v0.26.1 -o spdx-json > ragflow-image.spdx.json
syft infiniflow/ragflow:v0.26.1 -o cyclonedx-json > ragflow-image.cdx.json

# Container vulnerability scan
trivy image --scanners vuln,secret,misconfig infiniflow/ragflow:v0.26.1
# or
grype infiniflow/ragflow:v0.26.1

# Filesystem/source SBOM
cd /Users/ankitsingh/Documents/deep-wiki/ragflow
syft dir:. -o spdx-json > ../ragflow-artifacts/ragflow-source.spdx.json

# Python license/vulnerability examples
uv export --format requirements-txt > requirements-export.txt
pip-audit -r requirements-export.txt
pip-licenses --format=json --with-license-file --with-urls > python-licenses.json

# Node/frontend examples
cd web
npm audit --omit=dev
npx license-checker-rseidelsohn --json --out ../../ragflow-artifacts/web-licenses.json

# Go examples
cd ..
govulncheck ./...
go list -m -json all > ../ragflow-artifacts/go-modules.json
```

## License review acceptance criteria

- [ ] No GPL/AGPL/copyleft dependency included in a way that conflicts with organization policy.
- [ ] All commercial/proprietary SDK terms reviewed if used.
- [ ] All model/provider SDK licenses reviewed.
- [ ] All Docker image licenses reviewed.
- [ ] License obligations documented for Apache-2.0, MIT, BSD, MPL, LGPL, and other licenses present.
- [ ] Any unknown license resolved before production.

## Vulnerability review acceptance criteria

- [ ] No untriaged critical vulnerabilities in runtime images.
- [ ] No untriaged high vulnerabilities exposed to network, file parsing, auth, crypto, or sandbox paths.
- [ ] Parser/browser/file-processing CVEs triaged with priority.
- [ ] Vulnerabilities in disabled optional features documented as disabled and blocked by config/egress.
- [ ] Remediation owner and due date assigned for accepted risks.

## Build provenance requirements

- [ ] Build in enterprise CI.
- [ ] Use locked dependency files only.
- [ ] Mirror external dependencies internally where possible.
- [ ] Verify hashes for externally downloaded binaries/JARs/archives.
- [ ] Sign produced images or record provenance/attestation.
- [ ] Store SBOMs and scan reports with the release artifact.

## Current conclusion

The top-level project license is permissive and suitable for enterprise review, but the dependency and image surface is large enough that approval should require a generated SBOM, image scans, license report, and explicit triage of parser/browser/sandbox dependencies before production use. Two code-level items raised during this review should be treated as approval gates rather than generic dependency hygiene: (1) the unsafe-pickle `restricted_loads` path with a public PoC in `SECURITY.md` (require image ≥ commit `547b8cf9d` and tighten the allowlist — R21), and (2) third-party build binaries fetched without checksums (`office_oxide`). The committed `uv.lock` and several build steps also source from China-region mirrors (`pypi.tuna.tsinghua.edu.cn`, `goproxy.cn`, Aliyun/Huawei), which should be re-pointed at approved internal mirrors.
