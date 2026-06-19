# RAGFlow Egress Allowlist

## Policy

Production RAGFlow should run with **default-deny outbound egress**. Any destination not listed here should be blocked at the firewall, security group, Kubernetes NetworkPolicy, or egress proxy layer.

Separate **runtime egress** from **build/CI egress**. Runtime containers should not need access to package repositories, GitHub, Gitee, npm, PyPI, Maven, or OS package mirrors.

## Runtime allowlist template

Fill in exact hostnames, ports, owners, and approval tickets before production.

| Category | Destination | Port/protocol | Required? | Data sent | Controls / notes |
|---|---|---:|---|---|---|
| Enterprise LLM gateway | `TBD: llm-gateway.company.com` | TCP 443 HTTPS | Yes, if external model use is allowed | User prompts, retrieved chunks, system prompt, conversation context | Prefer one internal gateway instead of direct vendor egress. Enable DLP/logging if available. |
| Direct LLM provider | `TBD: api.openai.com / api.anthropic.com / etc.` | TCP 443 HTTPS | Only if gateway not used | Same as above | Must be separately approved per provider and model. |
| Embedding service | Local TEI `tei:80` or `TBD` | Internal HTTP or TCP 443 | Yes | Document chunks for embedding | Prefer local TEI/internal endpoint. |
| Rerank/OCR service | `TBD` | TCP 443 HTTPS | Optional | Query and retrieved text; possibly images/PDF snippets | Only if specific feature is approved. |
| Object storage | Local MinIO or `TBD: s3 endpoint` | Internal or TCP 443 | Yes | Source documents, parsed artifacts | Prefer private endpoint/VPC endpoint. |
| Identity provider | `TBD: idp.company.com` | TCP 443 HTTPS | If SSO/OIDC enabled | Auth redirects, tokens, user profile claims | Use enterprise IdP; restrict redirect URI. |
| SMTP relay | `TBD: smtp.company.com` | TCP 465/587/25 | If email enabled | User emails, verification/reset messages | Use approved relay only. |
| Langfuse (tracing) | `TBD: langfuse.company.com` | TCP 443 HTTPS | Optional | Prompt/response traces, model metadata | Real Python-side egress. **Configured per-tenant in the `tenant_langfuse` DB table at runtime, not in config** — any tenant admin with keys can enable it. There is no global default endpoint; control at the network layer. Use internal Langfuse only. |
| OpenTelemetry / OTLP | `TBD: otel-collector.company.com` | TCP 4318/443 | Optional | Traces/metrics | **Go admin/agent components only** (`go.mod` `go.opentelemetry.io/...`); the Python app has no OTel. No-op unless an OTLP exporter env var is set. Only relevant in Go/hybrid mode. |
| Approved document connector: Confluence/Jira | `api.atlassian.com`, `auth.atlassian.com` (Cloud), or tenant base URL | TCP 443 HTTPS | Optional | Connector credentials/tokens, document content | Hosts confirmed in `common/data_source/confluence_connector.py` / `config.py`. Approve per source. |
| Approved document connector: SharePoint/OneDrive/Teams/Outlook | `graph.microsoft.com`, `login.microsoftonline.com`, or private proxy | TCP 443 HTTPS | Optional | OAuth tokens, files/messages metadata/content | Approve per tenant and scope. |
| Approved document connector: Google Drive/Gmail | `www.googleapis.com`, `oauth2.googleapis.com`, or private proxy | TCP 443 HTTPS | Optional | OAuth tokens, document/email content | Approve per tenant and scope. |
| Approved document connector: other (~33 total) | per-source hosts, e.g. `slack.com`, `api.notion.com`, `api.dropboxapi.com`, `api.box.com`, `*.salesforce.com`, `*.zendesk.com`, GitHub/GitLab/Bitbucket, S3/WebDAV/Azure Blob | TCP 443 HTTPS | Optional | Connector credentials and files | RAGFlow ships ~33 connectors (`common/data_source/config.py`). Approve only the ones needed; block the rest. |
| Web search / crawl tools | `serpapi.com` (the "Google" tool backend), `api.firecrawl.dev` (Firecrawl plugin), Tavily, SearXNG, DuckDuckGo, ArXiv/PubMed | TCP 443 HTTPS | Optional | Query text, possibly retrieved context | Agent tools under `agent/tools/`. Note the "Google" tool egresses to `serpapi.com`, not Google directly. Disable unless approved. |
| DNS | Enterprise resolver | UDP/TCP 53 or DoT/DoH as approved | Yes | DNS queries | Prefer internal resolver with logging. |
| NTP | Enterprise NTP | UDP 123 | Yes | Time sync | Host-level preferred. |

## Explicit runtime blocks

Block these unless a separate business case is approved:

- `cloud.ragflow.io`, `ragflow.io` cloud features, and any hosted RAGFlow cloud endpoint. (Precautionary only — these appear in `README*.md` docs, not in runtime code.)
- **Chat-channel APIs.** The API server always loads 6 bundled channels (`feishu, discord, telegram, line, wecom, qqbot`) and a reconciler thread polls the `chat_channel` DB table every ~10s; a channel egresses once any tenant creates a bot via `POST /api/v1/chat-channels`. There is no global on/off flag, so block these hosts at the network layer and keep the table empty. Confirmed hardcoded hosts: `api.sgroup.qq.com`, `bots.qq.com` (QQ); `qyapi.weixin.qq.com` (WeCom); `api.telegram.org` (Telegram); `discord.com` (Discord); `api.line.me` / `api-data.line.me` (LINE); `open.feishu.cn` / `open.larksuite.com` (Feishu/Lark). Note WeCom/LINE/QQ also open **inbound** webhook listeners on the app host — keep them behind ingress/WAF.
- Arbitrary public web crawling/search from RAGFlow workers — including `serpapi.com` and `api.firecrawl.dev`.
- Package repositories and source hosts at runtime: PyPI, npm registry, GitHub, Gitee, Go proxy, Maven, Ubuntu mirrors, NodeSource, Microsoft package feeds, nginx.org. **Caveat:** `pip` is intentionally installed in the runtime venv (`Dockerfile` `ensurepip`), and Go/hybrid mode clones `infiniflow/resource` from GitHub/Gitee at startup (`docker/launch_backend_service.sh:220-222`); pre-bake the resource dir so blocking these does not break startup.
- Any user-supplied URL fetch that is not routed through an SSRF-aware proxy or allowlist.
- Direct database/storage/search access from outside the app/data network.

## Build/CI allowlist

Builds may need broader egress, but this should happen only in CI and preferably through internal artifact mirrors.

| Build source observed in repo | Why it appears | Recommendation |
|---|---|---|
| Docker Hub / image registries: `infiniflow/ragflow`, `infiniflow/ragflow_deps`, Elasticsearch, OpenSearch, MySQL, MinIO, Valkey, NATS, TEI | Container images | Mirror approved images internally and pin by digest. |
| Ubuntu package repositories | OS packages in Dockerfile | Use approved base image and internal apt mirror. |
| `https://nginx.org` | Nginx signing key + package in `Dockerfile` | Already uses a scoped `signed-by` keyring (good). Mirror internally or use approved distro package. |
| `https://deb.nodesource.com` | Node.js setup script in `Dockerfile:92` | **`curl -fsSL ... | bash -` executes a remote script as root.** Avoid curl-pipe-shell; mirror package or use an approved Node base image. |
| `https://packages.microsoft.com` | ODBC driver key/list in `Dockerfile:103-104` | **Uses deprecated `apt-key add -` over plain `curl` (no `-f`), adding a globally-trusted key.** Scope with `signed-by` keyring (as nginx does) or pre-bake; approve only if RDBMS connectors require it. |
| `https://github.com/infiniflow/resource.git` / `gitee.com/infiniflow/resource` | Infinity resources cloned in `Dockerfile:52-54` and `launch_backend_service.sh:220-222` (build **and** runtime) | Pin commit/hash and mirror internally; pre-bake the resource dir so runtime clone is not needed. |
| `github.com/yfedoseev/office_oxide` | `office_oxide` native lib (v0.1.2) downloaded in `build.sh:168`, linked into Go binaries via CGO | **Third-party GitHub account, no checksum verification** (`curl … | tar`). Vendor, pin by digest, and verify the prebuilt binary. |
| `github.com/explosion/spacy-models` | `en_core_web_sm` 3.8.0 wheel in `pyproject.toml:112` | Hash-pinned in `uv.lock` (good) but bypasses PyPI registry review — mirror internally. |
| PyPI mirror — `pypi.tuna.tsinghua.edu.cn` | Python deps in `uv.lock` (~3288 URLs vs 2 to `pypi.org`) | **`uv.lock` is resolved almost entirely against the Tsinghua (China) academic mirror.** `Dockerfile:162-167` sed-rewrites the host to `pypi.org`/`mirrors.aliyun.com` at build time (hashes preserved). Re-resolve the lock against an approved internal mirror; confirm host-swap-with-locked-hash is acceptable to policy. |
| npm registry | Frontend dependencies | Build from lockfile using internal npm proxy. |
| Go proxy — `goproxy.cn` (primary), `proxy.golang.org`, `direct` | Go deps in `build.sh:266-277` | China `goproxy.cn` is the first default proxy. Use internal Go proxy or vendored modules. |
| Maven Central / Huawei mirror | Tika server JAR in `download_deps.py` | Pin hash and mirror internally. |
| Chrome-for-testing / Google storage / npm mirror | Selenium/Chrome deps in `download_deps.py` | Pin hash and mirror internally. |
| OpenAI public blob for `cl100k_base.tiktoken` | Tokenizer asset (`download_deps.py`) | Pin hash and mirror internally. |
| `astral-sh/uv` (GitHub releases) | `uv` package manager binary in `download_deps.py` | Pin and mirror internally. |

## Enforcement options

### Kubernetes

- Put RAGFlow in a dedicated namespace.
- Use a default-deny egress NetworkPolicy.
- Add explicit egress rules for internal data services and approved external FQDNs via an egress gateway/proxy.
- Do not rely on pod-level FQDN rules unless your CNI supports and logs them reliably.

### Docker / VM

- Place containers on a private bridge network.
- Bind internal ports to localhost only, or remove host port mappings entirely.
- Route outbound HTTP/HTTPS through a controlled proxy using `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY`.
- Log proxy decisions and alert on denied destinations.

## Verification checklist

- [ ] Runtime containers cannot reach `github.com`, `pypi.org`, `registry.npmjs.org`, or arbitrary public sites.
- [ ] Runtime containers can reach only the approved LLM/embedding/storage/IdP/observability endpoints.
- [ ] DNS logs show no unexpected recurring outbound domains.
- [ ] Egress proxy logs are retained and reviewed during pilot.
- [ ] Session sharing, cloud endpoints, and chat-channel connectors are disabled unless explicitly approved.
