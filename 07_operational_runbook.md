# RAGFlow Operational Runbook

This runbook is optional for initial architecture approval, but recommended before pilot or production.

## Ownership

| Role | Owner | Backup | Notes |
|---|---|---|---|
| Application owner | TBD | TBD | Business owner for RAGFlow use case |
| Platform owner | TBD | TBD | Runtime, compute, ingress, secrets |
| Security owner | TBD | TBD | Security exceptions, scans, incident response |
| Data owner | TBD | TBD | Approved datasets and retention |
| Model owner | TBD | TBD | LLM/embedding provider approval |

## Environments

| Environment | Purpose | Data allowed | Egress posture |
|---|---|---|---|
| Dev | Engineering validation | Synthetic/non-sensitive only | Restricted but may allow build egress |
| Pilot | Limited users/use cases | Approved internal data only | Runtime default-deny allowlist |
| Prod | General approved use | Approved data classes only | Runtime default-deny allowlist |

## Standard operating procedures

### Deployment

1. Confirm approved image digest and SBOM are available (image ≥ deserialization-fix commit `547b8cf9d`).
2. Confirm secrets are provisioned in secrets manager; the reused default `infini_rag_flow` passwords are replaced with distinct per-service values and `RAGFLOW_SECRET_KEY` is set.
3. Change the default admin login `admin@ragflow.io` / `admin`; set `REGISTER_ENABLED=0`; disable the admin server (`--enable-adminserver`) or restrict 9381 to admin VPN.
4. Confirm egress allowlist is deployed (LLM/embedding/connector/Langfuse hosts; chat-channel and package-repo hosts blocked).
5. Confirm sandbox/code-executor is disabled unless separately approved, `local`/`ssh` providers are forbidden, and host port mappings for the data tier/admin are removed or bound to `127.0.0.1`.
6. Deploy app and private data services.
7. Verify only the ingress endpoint is reachable from user networks (no `0.0.0.0` data-tier/admin ports).
8. Run smoke tests: login, upload test document, index, query, delete.
9. Review egress logs during smoke test.

### Backup

- Back up MySQL metadata DB.
- Back up object storage bucket or MinIO volume.
- Back up search/vector index snapshots.
- Back up configuration needed to reconstruct providers/connectors without exporting secrets into plain text.
- Record backup frequency, retention, and encryption controls.

### Restore test

At least once before production:

1. Restore MySQL to a clean environment.
2. Restore object storage.
3. Restore or rebuild vector/search index.
4. Validate documents, chunks, and retrieval behavior.
5. Validate access controls after restore.
6. Record RTO/RPO achieved.

### Patch and upgrade

- Track upstream RAGFlow releases and security advisories.
- Rebuild images on base image or high/critical dependency fixes.
- Regenerate SBOM and scans for each release.
- Test upgrade in dev/pilot before production.
- Keep a rollback plan and database backup before upgrade.

### Secret rotation

Rotate at minimum:

- MySQL password.
- Redis/Valkey password.
- MinIO/S3 keys.
- Elasticsearch/OpenSearch credentials.
- LLM provider keys.
- Connector OAuth client secrets and service account keys.
- SMTP credentials.

Rotation procedure:

1. Create new secret.
2. Update secrets manager/config.
3. Restart affected services.
4. Validate functionality.
5. Revoke old secret.
6. Confirm old secret no longer works.

### User onboarding/offboarding

- Onboarding must go through SSO/group membership or admin approval.
- Grant least privilege needed for dataset/model/provider administration.
- Offboarding must revoke user access, API keys, connector tokens, and admin roles.
- Review stale API keys at least monthly during pilot.

## Monitoring and alerts

Minimum alerts:

- Failed login spike.
- Admin login or admin config change.
- New model provider/API key added.
- New connector added or connector scope changed.
- Egress denied events.
- Egress to non-approved destination.
- Large document upload volume spike.
- Parser/worker error spike.
- LLM token/cost spike.
- DB/storage/search capacity thresholds.
- Sandbox startup attempt if sandbox is supposed to be disabled.

## Incident response

### Suspected unapproved egress

1. Block destination at egress proxy/firewall.
2. Preserve proxy, DNS, app, and ingress logs.
3. Identify user/session/task and data involved.
4. Disable relevant connector/model/provider key.
5. Notify security and data owner.
6. Rotate exposed credentials if applicable.
7. Document root cause and update allowlist/hardening controls.

### Suspected credential leak

1. Revoke/rotate affected credential.
2. Search logs and configs for exposed values.
3. Identify blast radius.
4. Review access logs at provider/source system.
5. Patch logging/config issue that exposed the secret.

### Malicious document or parser compromise

1. Quarantine uploaded file and related task artifacts.
2. Stop ingestion workers if active compromise is suspected.
3. Preserve container/image/log evidence.
4. Patch parser dependencies or disable affected file type.
5. Re-index from trusted source after remediation.

## Change management

Require change approval for:

- Adding a new LLM/model provider.
- Enabling external connector categories.
- Enabling chat channels.
- Enabling MCP server.
- Enabling sandbox/code execution.
- Changing retention periods.
- Exposing new ports/services.
- Changing egress allowlist.
- Upgrading major RAGFlow version or document engine.

## Monthly pilot review

- Review users and API keys.
- Review connector list and scopes.
- Review egress logs for unexpected destinations.
- Review vulnerability scan deltas.
- Review storage growth and retention/deletion effectiveness.
- Review incidents, support tickets, and model-cost anomalies.
