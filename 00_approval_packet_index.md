# RAGFlow Security Approval Packet

**Project reviewed:** `/Users/ankitsingh/Documents/deep-wiki/ragflow`  
**Upstream:** `https://github.com/infiniflow/ragflow.git`  
**Observed version:** `0.26.1` (confirmed in `pyproject.toml` and `docker/.env`)  
**Observed commit:** `065797b047573637c09af771055c723c8b00ecd9` (matches local `HEAD`)  
**License:** Apache-2.0 (confirmed in `LICENSE`)  
**Artifact status:** Reviewed and corrected against the source code at the commit above. Every factual claim in this packet was checked against repository files; see `09_reviewer_verification_log.md` for the evidence trail and remaining `TBD` items.

## Executive position

RAGFlow appears to be a legitimate open-source project, but it should be reviewed as a **self-hosted AI infrastructure platform**, not as a small local CLI tool. Approval should be conditional on network egress controls, explicit connector/model choices, disabled code execution unless separately approved, and hardened deployment defaults.

**The shipped defaults are not safe for any non-local deployment.** Verified against the code, the out-of-the-box `docker compose` stack:

- Seeds a default admin login **`admin@ragflow.io` / `admin`** (`admin/server/auth.py:90-99`) and enables the admin server by default (`docker/docker-compose.yml` ships `--enable-adminserver` uncommented).
- Uses the **same password `infini_rag_flow`** for MySQL, Redis, MinIO, Elasticsearch, OceanBase and SeekDB (OpenSearch uses `infini_rag_flow_OS_01`) — `docker/.env`.
- **Publishes the data tier and admin/API ports to `0.0.0.0`** (MySQL 3306, Redis 6379, MinIO 9000/9001, Elasticsearch 1200, API 9380, admin 9381, MCP 9382, Go 9383/9384) — no `127.0.0.1` binding anywhere in the compose files.
- Enables **open self-registration** (`REGISTER_ENABLED=1`) and password login (`DISABLE_PASSWORD_LOGIN=false`).
- Ships a **latent unsafe-pickle deserialization path** (`api/utils/configs.py` `restricted_loads`) whose `numpy` allowlist reaches `numpy.f2py.diagnose.run_command` (RCE), publicly documented with a working PoC in the repo's own `SECURITY.md`.

These are corrected throughout the packet below and must be remediated before pilot.

## Files in this packet

This packet contains **10 files** (`00`–`09`):

0. [`00_approval_packet_index.md`](./00_approval_packet_index.md) — this index, executive position, and approval conditions.
1. [`01_architecture_data_flow.md`](./01_architecture_data_flow.md) — architecture, trust boundaries, data flow, and Mermaid diagrams.
2. [`02_egress_allowlist.md`](./02_egress_allowlist.md) — runtime and build-time egress allowlist template.
3. [`03_deployment_hardening_checklist.md`](./03_deployment_hardening_checklist.md) — hardening checklist for Docker/Kubernetes/self-hosted deployment.
4. [`04_threat_model_risk_register.md`](./04_threat_model_risk_register.md) — threat model and risk register.
5. [`05_data_handling_privacy.md`](./05_data_handling_privacy.md) — data classification, storage, retention, and privacy notes.
6. [`06_sbom_dependency_license_review.md`](./06_sbom_dependency_license_review.md) — SBOM, dependency, license, and supply-chain review plan.
7. [`07_operational_runbook.md`](./07_operational_runbook.md) — optional but recommended operational runbook.
8. [`08_reviewer_handoff_summary.md`](./08_reviewer_handoff_summary.md) — handoff summary describing how the packet was produced and what the reviewer should focus on.
9. [`09_reviewer_verification_log.md`](./09_reviewer_verification_log.md) — code-grounded verification log: what was checked, the file:line evidence, and the corrections applied.

## Approval conditions recommended

- Deny outbound egress by default; allow only reviewed LLM, embedding, identity, storage, observability, and connector endpoints.
- Disable sandbox/code-executor and privileged Docker-socket access unless separately approved. Note RAGFlow also ships an in-product agent `execute_code` tool and a `local` sandbox provider that runs code on the app host with **no isolation** — both must be controlled, not just the optional Docker sandbox service (see `04`/`03`).
- Disable unneeded external connectors (~33 are bundled) and chat channels (6 are always loaded by the API server; see `02`).
- Use pinned images and internally mirrored dependencies for production builds. Run an image that includes the deserialization fix (commit `547b8cf9d` or later) **and** tighten the `restricted_loads` allowlist (`06`).
- Do not expose MySQL, Redis/Valkey, MinIO, Elasticsearch/OpenSearch, Infinity, OceanBase/SeekDB, NATS, sandbox executor (9385), MCP (9382), Go (9383/9384) or admin (9381) services directly to broad networks. **In the default compose these are all published to `0.0.0.0`** — remove the host mappings or rebind to `127.0.0.1`.
- Replace all default passwords (the shipped value is the single reused string `infini_rag_flow`) and the default admin login `admin@ragflow.io` / `admin`; store secrets in an enterprise secrets manager; set an explicit `RAGFLOW_SECRET_KEY`.
- Set `REGISTER_ENABLED=0` to disable open self-registration; route user access through TLS and SSO/OIDC where possible (only OAuth2/OIDC/GitHub are supported — there is **no SAML**).
- Run SCA/SBOM, image scan, and license review before production.

## Reviewer decisions still needed

- Which LLM provider or internal LLM gateway is approved?
- Are external embeddings/reranking/OCR allowed, or must they be local?
- Which document sources/connectors are required on day one?
- Is the agent code-execution feature required? If yes, who owns a separate sandbox review?
- What data classifications are allowed in RAGFlow initially?
- What retention period applies to source documents, parsed chunks, embeddings, chat logs, and backups?
