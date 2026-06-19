# RAGFlow Approval Packet — Reviewer Verification Log

## Purpose

This log records the independent verification of the approval packet against the RAGFlow source code. Its goal is to let an architect/security reviewer **audit the packet against the code instead of taking it on trust** — the exact concern (from `08`) that caused a prior project to be rejected for looking "shoddy" and not clearly mapped.

Every load-bearing factual claim in artifacts `00`–`08` was checked against repository files. Where a claim was vague, wrong, or omitted a material fact, the packet was corrected and the change is noted below with file:line evidence.

## Scope and method

- **Repository:** `/Users/ankitsingh/Documents/deep-wiki/ragflow` (upstream `github.com/infiniflow/ragflow`).
- **Commit verified:** `065797b047573637c09af771055c723c8b00ecd9` (matches local `HEAD`).
- **Version:** `0.26.1` (`pyproject.toml:3`, `docker/.env` `RAGFLOW_IMAGE=infiniflow/ragflow:v0.26.1`). **License:** Apache-2.0 (`LICENSE`).
- **Method:** claims were grouped into eight categories (ports/network, sandbox/code-exec, default credentials/auth, runtime egress, supply chain, `SECURITY.md`/pickle, config/architecture, cross-artifact consistency) and each was checked by reading the cited source files (`Read`/`Grep`/`git`). Findings requiring a security judgement were cross-checked against the code a second time before being written into the packet.
- **Limitation:** this is a static source review at one commit. It is **not** a running-system test, a generated SBOM, a CVE scan, or a penetration test — those remain required before production (see `06`). Endpoints marked `TBD` in `02` are deployment decisions, not facts to verify.

## Overall verdict

The packet's structure and most repo-derived facts were accurate. Core service ports (80/443, 9380, 9381, 9382, 9383/9384, 3306, 6379, 9000/9001, ES/OS 1200/1201), the Apache-2.0 license, the version/commit, the sandbox `privileged`+docker.sock claim, and the `SECURITY.md` write-up all **confirmed**. The material gaps were (a) the packet described several *recommended* states as if they were the *defaults* (admin server, port exposure, registration), (b) it stated generic risks where the code has concrete, nameable facts (exact passwords, exact hosts, an actual RCE path), and (c) it missed three code-execution paths and the always-on chat-channel subsystem. All are corrected.

## Confirmed claims (spot list)

| Claim | Evidence |
|---|---|
| Sandbox `sandbox-executor-manager` runs `privileged: true` with `/var/run/docker.sock` mounted | `docker/docker-compose-base.yml:162,167` |
| API 9380 / admin 9381 / MCP 9382 / Go 9383/9384 ports | `docker/.env:155-159`; `docker/docker-compose.yml:35-39` |
| ES host port 1200, OpenSearch 1201 | `docker/.env:38,45`; `docker-compose-base.yml:13,47` |
| `graspologic` Gitee git dependency | `pyproject.toml:55` |
| Build downloads: nginx.org, NodeSource, packages.microsoft.com, infiniflow/resource (gitee+github), OpenAI tiktoken blob, Chrome-for-testing, Tika JAR, uv | `Dockerfile:52-54,62,92,103-104`; `download_deps.py:24-44` |
| 64 LLM providers / 60 model files | `conf/llm_factories.json`; `conf/models/*.json` |
| Two-process runtime (API server + task executor), Go hybrid mode, admin & MCP servers | `api/ragflow_server.py`, `rag/svr/task_executor.py`, `cmd/*.go`, `admin/server/`, `mcp/server/` |
| `service_conf.yaml.template` controls DB/storage/OAuth/SMTP/LLM | `docker/service_conf.yaml.template` |
| `06` is a *plan* and never claims completed scans | `06` text — verified, no fabricated scan output |

## Corrections applied

### Defaults were described as the hardened target, not the shipped state

| Artifact claim (before) | Reality (verified) | Evidence |
|---|---|---|
| Admin server "internal/admin only … disabled if not needed" | **Enabled by default**; `--enable-adminserver` is uncommented and port 9381 published to `0.0.0.0` | `docker/docker-compose.yml:29-39` |
| MySQL/Redis/MinIO "private only" | **Always published to `0.0.0.0`** (no profile gate, no `127.0.0.1` bind) | `docker-compose-base.yml:201,241,217-219` |
| "Infinity/OceanBase/SeekDB/NATS/TEI ports vary" | Exact defaults: Infinity 23817/23820/5432, OceanBase 2881, SeekDB 2881, NATS 4222 + **8222 (hardcoded monitoring)**, TEI 6380, Kibana 6601 | `docker/.env:71-73,78,99,149,195,61`; `docker-compose-base.yml:85-87,118,142,258-259` |
| Sandbox port not listed | Sandbox executor listens on **9385** | `docker-compose-base.yml:164`; `docker/.env:275` |
| "Public registration disabled unless approved" (no default stated) | **`REGISTER_ENABLED=1`** (open signup) and `DISABLE_PASSWORD_LOGIN=false` | `docker/.env:243,306`; `common/settings.py:228` |

### Generic statements replaced with concrete facts

- **Default credentials.** One reused string `infini_rag_flow` covers MySQL, Redis, MinIO, Elasticsearch, OceanBase, SeekDB; OpenSearch uses `infini_rag_flow_OS_01`; MinIO user `rag_flow`, MySQL `root` (both root accounts). `docker/.env:42,52,82,103,111,138,146`; `docker-compose-base.yml:191,222-223`.
- **Default admin login.** `admin@ragflow.io` / `admin`, seeded by the admin server's `init_default_admin()` and by `init_superuser`. `admin/server/auth.py:90-99`; `api/db/init_data.py:45-46`.
- **Signing key.** `RAGFLOW_SECRET_KEY` is unset and auto-generated into Redis at runtime if absent. `common/settings.py:192-195`.
- **Connector hosts.** ~33 connectors in `common/data_source/config.py`; Confluence/Jira → `api.atlassian.com`/`auth.atlassian.com` (`confluence_connector.py:253`, `config.py:248`); plus SharePoint/Graph, Google, Slack, Notion, Dropbox, Box, Salesforce, etc.
- **Chat-channel hosts.** `api.sgroup.qq.com`/`bots.qq.com` (`qqbot/channel.py:19-20`), `qyapi.weixin.qq.com` (`wecom/channel.py:20`), plus Telegram/Discord/LINE/Feishu.
- **Web-search egress.** The "Google" tool uses **SerpApi** (`agent/tools/google.py:20`), and a Firecrawl plugin defaults to `api.firecrawl.dev` (`tools/firecrawl/firecrawl_config.py:16`).

### Incorrect claims fixed

| Claim (before) | Verdict | Evidence / correction |
|---|---|---|
| Chat channels "blocked/disabled by default" | **Incorrect** | API server always starts a reconciler thread loading 6 bundled channels; egress begins once a tenant adds a bot row. `api/ragflow_server.py:163-164`; `api/channels/bootstrap.py:36`. Corrected to "always-loaded subsystem; control at network/DB layer." |
| `service_conf.yaml.template` controls "observability-related integrations" | **Incorrect** | No observability keys in that file; Langfuse is per-tenant in the `tenant_langfuse` DB table (`api/db/db_models.py:854`). Corrected in `01`/`04`/`05`. |
| "Langfuse/OpenTelemetry" Python observability egress | **Imprecise** | Langfuse is real Python egress (`pyproject.toml:63`, `dialog_service.py:570-572`); OpenTelemetry/OTLP exists only in the Go components (`go.mod`) and is a no-op without an exporter. |
| `SECURITY.md` "appears inconsistent … ask owner to clarify" | **Understated** | It is a public PoC for a still-present RCE: `restricted_loads` allows any `numpy` attribute → `numpy.f2py.diagnose.run_command`. Code at `api/utils/configs.py:22-39` (the `__init__.py#L215` link in `SECURITY.md` is stale). Commit `547b8cf9d` removed the insecure default but left the allowlist. Now tracked as **R21**. |
| Gitee dependency mitigation "Pin commit" | **Imprecise** | Already pinned to commit `38e680cab…` in `pyproject.toml:55` and `uv.lock`; residual action is mirror + license review. |
| `00` index lists 7 files | **Incorrect/incomplete** | Did not list itself or `08`. Now lists all 10 files. |

## New risks added to the register

- **R21 — Unsafe pickle deserialization** (`restricted_loads`, `api/utils/configs.py:22-39`). Latent today (no in-tree `SerializedField(PICKLE)` column; reached only via `deserialize_b64`) but an acknowledged-unfixed RCE if attacker-controlled pickled bytes ever reach a PICKLE column. Require image ≥ `547b8cf9d`; tighten the allowlist.
- **R22 — In-product code execution beyond the Docker sandbox.** The agent `execute_code` tool (`agent/tools/code_exec.py`) and the `local`/`ssh` sandbox providers (`agent/sandbox/providers/local.py` — "not a sandbox boundary") run user code on the app host. Selecting `provider_type=local` is host-RCE in `ragflow-server`.

## Remaining `TBD` / reviewer decisions (not defects)

The `TBD` markers in `02` (egress allowlist) are intentional deployment inputs the reviewer must supply: the approved LLM gateway/provider, embedding/rerank/OCR path, identity provider, SMTP relay, observability endpoint, and which connectors are in scope. The open decisions listed in `00` and `08` (data classes, retention, sandbox/MCP posture, SBOM ownership) likewise remain with the reviewer. This packet makes the facts precise; it does not pre-empt those policy choices.
