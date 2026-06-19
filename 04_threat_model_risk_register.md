# RAGFlow Threat Model and Risk Register

## System summary

RAGFlow is a self-hosted retrieval-augmented generation platform. It ingests documents from uploads or connectors, parses and chunks content, stores raw documents and embeddings, retrieves relevant context, and sends prompts/context to configured LLM services.

## Assets to protect

- Source documents and uploaded files.
- Parsed text, tables, images, chunks, and embeddings.
- User prompts, model responses, chat history, and citations.
- API keys and connector OAuth tokens.
- User identities, roles, and tenant metadata.
- Database, object store, vector/search indexes, Redis/Valkey queues, logs, and backups.
- Administrative interfaces and service configuration.

## Threat actors

- External attacker reaching an exposed service.
- Authenticated but unauthorized internal user.
- Malicious or compromised connector/data source.
- Malicious document attempting parser exploit or prompt injection.
- Compromised LLM/provider/observability endpoint.
- Supply-chain attacker via package, image, git dependency, or binary download.
- Misconfiguration or accidental egress exposure.

## Trust boundaries

1. User/admin browser to RAGFlow ingress.
2. RAGFlow application containers to private data tier.
3. RAGFlow application containers to external LLM/connector/observability endpoints.
4. Optional sandbox/code executor boundary.
5. Build/CI boundary for dependency and image acquisition.

## Assumptions

- RAGFlow will be self-hosted, not using RAGFlow Cloud.
- Runtime egress will be default-deny.
- Code execution is disabled unless separately approved.
- Only approved connectors and model providers are configured.
- Production secrets are managed outside the repository.

## Risk register

| ID | Risk | Severity | Likelihood | Mitigation | Status |
|---|---|---:|---:|---|---|
| R1 | Sensitive documents or retrieved chunks are sent to unapproved LLM providers. | High | Medium | Use enterprise LLM gateway or explicit provider allowlist; document data classes; log egress. | Open |
| R2 | Runtime makes unexpected outbound calls to public internet. | High | Medium | Default-deny egress; allowlist exact destinations; monitor DNS/proxy logs. | Open |
| R3 | Optional Docker sandbox executor escapes or abuses Docker socket/privileged container. | Critical | Medium | Disable by default (profile `sandbox`, off). If used: note `privileged:true` + `/var/run/docker.sock` = host root, and `no-new-privileges` is negated by `privileged`; require host gVisor (`runsc`), set `SANDBOX_ENABLE_SECCOMP=true`, isolate node/VM, deny sandbox egress, don't expose 9385. | Open |
| R4 | Default Docker Compose exposes MySQL, Redis, MinIO, Elasticsearch, admin/MCP/Go ports — all bound to `0.0.0.0`. | High | High | MySQL(3306)/MinIO(9000/9001)/Redis(6379) are **always** published; the search engine is published for the selected `DOC_ENGINE` profile (ES/1200 default); admin 9381 is on by default. Remove host mappings or bind `127.0.0.1`; private subnets only; ingress only for UI/API. | Open |
| R5 | Default passwords in `docker/.env` are used in production. | High | High | Single reused value `infini_rag_flow` covers MySQL/Redis/MinIO/Elasticsearch/OceanBase/SeekDB (OpenSearch `infini_rag_flow_OS_01`); MinIO/MySQL are root accounts. Default admin login `admin@ragflow.io` / `admin`. Replace all with distinct secrets; set `RAGFLOW_SECRET_KEY`; rotation test. | Open |
| R6 | Malicious PDF/Office/image exploits parser stack such as Tika, Chrome/Selenium, OCR, or document libraries. | High | Medium | Patch images; scan dependencies; isolate ingestion worker; limit file types/sizes; malware scan uploads. | Open |
| R7 | Prompt injection in ingested documents causes data exfiltration or unsafe tool use. | High | Medium | Treat retrieved content as untrusted; disable risky tools; limit connectors; model/tool guardrails; monitor unusual requests. | Open |
| R8 | SSRF through URL ingestion, connectors, web fetch, or file download paths. | High | Medium | Route through SSRF-aware proxy; block RFC1918/link-local/cloud metadata; allowlist connector hosts. | Open |
| R9 | Langfuse traces leak prompts, document chunks, or responses to an external observability service. | High | Low/Medium | Langfuse is a real Python egress configured **per-tenant in the `tenant_langfuse` DB table** (no config gate), so any tenant admin with keys can enable it — control at the network layer; use internal Langfuse only; redact. (OpenTelemetry/OTLP exists only in the Go admin/agent components and is a no-op unless an exporter is set.) | Open |
| R10 | Connector tokens over-scoped or reused across tenants. | High | Medium | Least privilege OAuth scopes; per-tenant service accounts; token encryption; audit connector access. | Open |
| R11 | External chat channels leak enterprise conversations to Discord/Telegram/Line/Feishu/WeCom/QQ. | High | Medium | The API server **always loads 6 bundled channels** and a reconciler polls the `chat_channel` DB table every ~10s (no global off flag); WeCom/LINE/QQ also open inbound webhooks. Keep the table empty, restrict `POST /api/v1/chat-channels`, and block egress to the hardcoded hosts (see `02`). | Open |
| R12 | Supply-chain compromise through PyPI/npm/Go/Docker/GitHub/Gitee/binary downloads. | High | Medium | Generate SBOM; internal mirrors; pin digests/hashes; scan images; review git deps. | Open |
| R13 | API/admin service exposed without sufficient auth or network restriction. | High | Medium | SSO; admin network/VPN; WAF/rate limits; no public admin port. | Open |
| R14 | Object store or search index contains recoverable sensitive data after user deletes source documents. | Medium/High | Medium | Define deletion workflow for raw docs, chunks, embeddings, backups; test deletion. | Open |
| R15 | Logs contain secrets, prompts, chunks, or connector data. | High | Medium | Log redaction; least log verbosity; restrict log access; retention policy. | Open |
| R16 | Model provider retains or trains on enterprise prompts. | High | Low/Medium | Contractual review; enterprise plan settings; gateway-based provider enforcement. | Open |
| R17 | MCP server expands tool surface and allows unexpected external calls or actions. | High | Low/Medium | Disable by default; approve MCP tools individually; require auth and network isolation. | Open |
| R18 | Public registration or weak tenant isolation allows unauthorized access. | High | Medium | Open self-registration is **on by default** (`REGISTER_ENABLED=1`) and password login on (`DISABLE_PASSWORD_LOGIN=false`); anyone reaching the UI/API can create an account. Set `REGISTER_ENABLED=0`; review tenant permissions; admin approval workflow. | Open |
| R19 | Unpatched vulnerabilities in large Python/JS/Go dependency tree. | High | Medium | SCA scans; patch cadence; vulnerability SLA; rebuild images regularly. | Open |
| R20 | Backup/restore failure causes data loss or inability to recover after incident. | Medium | Medium | Back up DB/object store/search index; test restore; document RTO/RPO. | Open |
| R21 | Unsafe pickle deserialization: `restricted_loads` (`api/utils/configs.py:22-39`) allows any `numpy` attribute, reaching `numpy.f2py.diagnose.run_command` (RCE). Documented with a working PoC in the repo's own `SECURITY.md`. | High | Low | **Latent today** (no in-tree DB column uses `SerializedField(PICKLE)`; reached only via `deserialize_b64`); detonates if attacker controls pickled bytes (DB write/SQLi, untrusted backup restore, compromised replica) AND a PICKLE column is read. Run image ≥ commit `547b8cf9d`; tighten `find_class` to an explicit symbol allowlist; block `numpy.f2py`; flip `SerializedField` default to JSON. | Open |
| R22 | In-product code execution beyond the Docker sandbox: agent `execute_code` tool and the `local`/`ssh` sandbox providers run user code on the app host with no isolation and no AST check. | High | Low/Medium | Selecting `provider_type=local` makes agent code execution host-RCE in `ragflow-server`. Forbid `local`/`ssh` providers; lock Admin → Sandbox Settings; govern who can author agents using `execute_code`. | Open |

## Approval blockers

The following should be resolved before production approval:

- R1/R2: LLM and general egress controls are not yet implemented.
- R3/R22: Sandbox/code-executor posture and the in-product `execute_code` tool / `local`-provider host-RCE path must be explicitly disabled or approved.
- R4/R5: Default `0.0.0.0` port exposure, the always-on admin server, the reused `infini_rag_flow` credentials, and the default `admin@ragflow.io` / `admin` login must be remediated.
- R18: Open self-registration (`REGISTER_ENABLED=1`) must be disabled for any shared deployment.
- R21: The unsafe-pickle deserialization path must be patched (image ≥ `547b8cf9d`) and the allowlist tightened.
- R12/R19: SBOM, image scan, and dependency review must be completed.
- R14/R20: Retention, deletion, backup, and restore process must be defined.

## Residual risk statement

Even with hardening, RAGFlow remains an AI data-processing platform that intentionally sends selected user prompts and retrieved context to configured model providers. The residual risk is acceptable only if the organization approves the specific model path, data classes, retention policy, and monitoring controls.
