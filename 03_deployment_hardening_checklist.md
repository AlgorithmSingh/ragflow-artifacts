# RAGFlow Deployment Hardening Checklist

Use this as the go/no-go checklist before submitting or deploying RAGFlow.

## 1. Architecture decisions

- [ ] Deployment mode selected: Docker Compose, Kubernetes, or managed platform.
- [ ] Document engine selected: Elasticsearch, OpenSearch, Infinity, OceanBase, or SeekDB.
- [ ] Storage selected: local MinIO, enterprise S3-compatible storage, or cloud object storage with private endpoint.
- [ ] Model path selected: enterprise LLM gateway, direct approved vendor, or local model.
- [ ] Embedding path selected: local TEI/internal embedding endpoint preferred.
- [ ] Required connectors listed; all others disabled or blocked by egress.
- [ ] Code-execution/sandbox decision recorded. Default: disabled.
- [ ] MCP server decision recorded. Default: disabled.

## 2. Network hardening

> **Default reality (verified):** the compose files publish ports to `0.0.0.0` (no `127.0.0.1` binding anywhere). Under the default `elasticsearch,cpu` profile the host-published set is: web 80/443, API 9380, admin 9381, MCP 9382, Go 9383/9384, Elasticsearch 1200, MySQL 3306, MinIO 9000/9001, Redis 6379. The admin server is **enabled by default** (`--enable-adminserver`). The data tier is host-reachable out of the box — the items below are remediation actions, not the current state.

- [ ] Only TLS ingress/reverse proxy is exposed to users.
- [ ] All compose port mappings rebound to `127.0.0.1` or removed; nothing data-tier/admin published to `0.0.0.0`.
- [ ] RAGFlow API `9380` is not directly exposed to broad networks.
- [ ] Admin server **disabled** (comment out `--enable-adminserver` in `docker-compose.yml`) or, if needed, port `9381` restricted to admin network/VPN. (On by default.)
- [ ] MCP `9382` host mapping removed (process is off by default, but the port is still published); Go admin `9383` / Go API `9384` host mappings removed unless Go/hybrid mode is used (published even when `API_PROXY_SCHEME=python`).
- [ ] Sandbox executor `9385` not exposed (off by default; only under the `sandbox` profile).
- [ ] MySQL `3306` is private only. **(Always published by default via `EXPOSE_MYSQL_PORT`.)**
- [ ] Redis/Valkey `6379` is private only. **(Always published by default.)**
- [ ] MinIO API/console `9000/9001` is private only. **(Always published; these are the storage root account.)**
- [ ] Elasticsearch `1200` / OpenSearch `1201` / Infinity `23817/23820/5432` / OceanBase `2881` / SeekDB `2881` / NATS `4222`+`8222` (monitoring) / TEI `6380` / Kibana `6601` ports are private only.
- [ ] Default-deny egress enforced.
- [ ] Egress allowlist implemented and logged.
- [ ] No outbound package-repository access from runtime containers (note `pip` is present in the runtime venv and Go mode clones `infiniflow/resource` at startup — pre-bake the resource dir).

## 3. Authentication and authorization

> **Default reality (verified):** `REGISTER_ENABLED=1` (open self-registration ON) and `DISABLE_PASSWORD_LOGIN=false`. The admin server seeds a default login **`admin@ragflow.io` / `admin`** (`admin/server/auth.py:90-99`); the API server's `init_superuser` uses the same defaults (`admin@ragflow.io` / `admin`, overridable via `DEFAULT_SUPERUSER_*`). Only OAuth2 / OIDC / GitHub SSO are supported — **there is no SAML**.

- [ ] Default admin login `admin@ragflow.io` / `admin` changed immediately after first start.
- [ ] Public self-registration disabled: `REGISTER_ENABLED=0`. (Enabled by default.)
- [ ] SSO configured via OAuth2/OIDC/GitHub if required (the `oauth:` block in `service_conf.yaml.template` is commented out by default). Do not assume SAML is available.
- [ ] Password login disabled (`DISABLE_PASSWORD_LOGIN=true`) if SSO-only deployment is required.
- [ ] Admin accounts are named users or tied to enterprise groups.
- [ ] RBAC/permission settings reviewed (`permission:` block in `service_conf.yaml.template`).
- [ ] API keys are scoped, rotated, and inventoried.

## 4. Secret management

> **Default reality (verified, `docker/.env`):** one reused password string covers six stores. Compromising any default exposes the rest.
>
> | Service | Default user | Default password | Notes |
> |---|---|---|---|
> | MySQL | `root` | `infini_rag_flow` | mapped to `MYSQL_ROOT_PASSWORD` |
> | Redis/Valkey | (none) | `infini_rag_flow` | |
> | MinIO | `rag_flow` | `infini_rag_flow` | mapped to `MINIO_ROOT_USER/PASSWORD` (storage root); console on 9001 |
> | Elasticsearch | `elastic` | `infini_rag_flow` | |
> | OpenSearch | `admin` | `infini_rag_flow_OS_01` | |
> | OceanBase | `root@ragflow` | `infini_rag_flow` | |
> | SeekDB | `root` | `infini_rag_flow` | |
>
> The JWT/session `RAGFLOW_SECRET_KEY` is **not** set in `.env`; if unset it is auto-generated at runtime and stored in Redis (`common/settings.py:192-195`), so its confidentiality depends on Redis (which has the default password above) and it is not under change control.

- [ ] Every default password above replaced with a **distinct** per-service secret (do not reuse one string).
- [ ] Explicit `RAGFLOW_SECRET_KEY` (≥32 chars) set so the signing key is not auto-generated into Redis.
- [ ] Secrets are stored in an enterprise secrets manager, not committed files.
- [ ] `.env`, service config, and logs are protected from broad read access.
- [ ] LLM provider keys are tenant-scoped and never shared across environments.
- [ ] Connector OAuth secrets and tokens are encrypted at rest.
- [ ] Secret rotation procedure tested.

## 5. Container and runtime hardening

- [ ] Images pinned by immutable digest, not just tag.
- [ ] No `latest` images in production.
- [ ] Images scanned with Trivy/Grype or approved scanner.
- [ ] Base image and OS packages are patched.
- [ ] Runtime containers run with least privilege where supported.
- [ ] Linux capabilities are dropped where supported.
- [ ] Writeable filesystem paths are minimized.
- [ ] Container logs do not include secrets or raw sensitive prompts.
- [ ] Resource limits set for CPU/memory/storage.
- [ ] Health checks configured.

## 6. Sandbox/code-executor hardening

Default recommendation: **do not deploy the sandbox/code-executor feature**.

> **Three code-execution paths exist (verified) — disabling the Docker sandbox service alone does not cover all of them:**
> 1. **Docker sandbox service** (`docker-compose-base.yml:158-184`, profile `sandbox`, off by default): `privileged: true` + `/var/run/docker.sock` mount = effective host root. `security_opt: no-new-privileges:true` is set on the manager but **negated by `privileged`**. Inner per-task containers need gVisor (`--runtime=runsc`) which the image does **not** install (must be on the host, else creation fails/degrades), and `SANDBOX_ENABLE_SECCOMP=false` by default. Listens on **9385**.
> 2. **In-product agent tool `execute_code`** (`agent/tools/code_exec.py`, `CodeExec` component): lets any agent/workflow author run arbitrary Python/JS via the configured provider. Referenced by built-in agent templates. Available in the agent builder regardless of the Docker service.
> 3. **`local` / `ssh` sandbox providers** (`agent/sandbox/providers/local.py`): run user code as a child process **on the RAGFlow app host with no container boundary and no AST safety check** (the denylist runs only on the `self_managed` HTTP path). If an admin selects `provider_type=local` in Admin → Sandbox Settings, agent code execution becomes host-level RCE in `ragflow-server`.

If code execution is required:

- [ ] Separate architecture/security review completed.
- [ ] Provider restricted to the containerized `self_managed` path; **`local` and `ssh` providers forbidden in production**; Admin → Sandbox Settings locked down.
- [ ] In-product `execute_code` agent tool governed (restrict who can author agents that use it).
- [ ] No broad Docker socket mount in production unless explicitly accepted.
- [ ] `privileged: true` exception approved by security (note `no-new-privileges` is negated by it).
- [ ] gVisor (`runsc`) installed and configured as a Docker runtime on the sandbox host (not provisioned by the image).
- [ ] `SANDBOX_ENABLE_SECCOMP=true` set (defaults to `false`).
- [ ] Sandbox runs in isolated node pool or VM boundary.
- [ ] Egress from sandbox is denied by default.
- [ ] CPU, memory (`SANDBOX_MAX_MEMORY`, default 256m), process/pids, file, and wall-clock (`SANDBOX_TIMEOUT`, default 10s) limits enforced — note CPU-share and `--pids-limit` are **not** set on the inner `docker run` by default.
- [ ] Pre-execution AST denylist treated as defense-in-depth only (bypassable; not applied to non-`self_managed` providers).
- [ ] Sandbox port 9385 not host-exposed.
- [ ] Temporary artifacts are deleted after retention period (`SANDBOX_ARTIFACT_EXPIRE_DAYS`, default 7).
- [ ] Sandbox images are scanned and pinned (defaults are `:latest`).
- [ ] Abuse monitoring and kill switch implemented.

## 7. Connector and external integration hardening

- [ ] Only approved connectors enabled.
- [ ] Connector OAuth scopes minimized.
- [ ] Service accounts are least privilege and read-only where possible.
- [ ] Connector sync frequency and volume limits set.
- [ ] External chat channels disabled unless approved.
- [ ] Web crawling/search tools disabled unless approved.
- [ ] SSRF protections reviewed for URL ingestion and connector downloads.

## 8. LLM/data egress controls

- [ ] LLM traffic uses enterprise gateway or approved provider endpoint.
- [ ] Prompt logging policy defined.
- [ ] Retrieved document chunks sent to LLM are limited to necessary context.
- [ ] Sensitive data classes allowed in prompts are documented.
- [ ] Provider retention/training settings confirmed contractually.
- [ ] Langfuse/OTel tracing disabled by default or routed to internal systems only.

## 9. Storage, backup, and encryption

- [ ] MySQL encrypted at rest and backed up.
- [ ] Object storage encrypted at rest and access-logged.
- [ ] Vector/search index encrypted at rest where supported.
- [ ] Redis/Valkey is password-protected and private.
- [ ] Backup schedule defined for DB, object store, and search index.
- [ ] Restore test completed.
- [ ] Data deletion workflow tested for documents, chunks, embeddings, and chat records.

## 10. Logging, monitoring, and audit

- [ ] Application, proxy, and egress logs collected centrally.
- [ ] Admin actions are logged.
- [ ] Authentication failures and API key use are monitored.
- [ ] Egress denied events are monitored.
- [ ] Storage access logs are monitored.
- [ ] Alerts defined for abnormal upload, ingestion, LLM, and connector activity.
- [ ] Log retention matches enterprise policy.

## 11. Supply-chain controls

- [ ] SBOM generated for Python, Node, Go, and container images.
- [ ] License review completed.
- [ ] High/critical vulnerabilities triaged or fixed.
- [ ] Running an image that includes the deserialization fix commit `547b8cf9d` or later (so `deserialize_b64` always routes through `restricted_loads` — pre-fix images do a bare `pickle.loads`), **and** the `restricted_loads` `numpy` allowlist has been tightened (see `06` / R21).
- [ ] External binary downloads are pinned by hash and mirrored internally (note `office_oxide` from `github.com/yfedoseev` has no checksum; `en_core_web_sm` wheel is hash-pinned).
- [ ] Git dependencies are pinned by commit and mirrored internally (`graspologic` is already commit-pinned to a Gitee SHA; mirror it).
- [ ] `uv.lock` re-resolved against an approved internal mirror (committed lock points at `pypi.tuna.tsinghua.edu.cn`; `Dockerfile` sed-swaps the host at build time).
- [ ] CI build uses locked dependencies only.

## 12. Production acceptance criteria

RAGFlow is ready for pilot only when:

- [ ] Architecture and data-flow diagram is reviewed.
- [ ] Egress allowlist is implemented and tested.
- [ ] Sandbox/code-executor disabled, **and** `local`/`ssh` providers forbidden, **and** in-product `execute_code` tool governed.
- [ ] Default credentials removed: all `infini_rag_flow` store passwords rotated to distinct secrets; default admin `admin@ragflow.io` / `admin` changed; `RAGFLOW_SECRET_KEY` set.
- [ ] Open registration disabled (`REGISTER_ENABLED=0`) and admin server disabled or VPN-restricted.
- [ ] Internal services are not publicly reachable (no `0.0.0.0` host port bindings for the data tier / admin).
- [ ] Deserialization fix present (image ≥ commit `547b8cf9d`) and `restricted_loads` allowlist tightened.
- [ ] SCA/image/license scans are complete.
- [ ] Data handling policy is approved.
- [ ] Backup/restore and incident response paths are documented.
