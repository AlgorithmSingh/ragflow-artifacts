# RAGFlow Data Handling and Privacy Note

## Purpose

This artifact explains what data RAGFlow processes, where it is stored, and what may leave the environment. It is intended for privacy, security, architecture, and data-owner review.

## Data categories

| Data category | Examples | Sensitivity concern |
|---|---|---|
| Source documents | PDFs, DOCX, PPTX, spreadsheets, emails, web pages, connector-synced files | May contain confidential, regulated, personal, customer, or employee data |
| Parsed content | Extracted text, tables, images, metadata, document chunks | Often equivalent sensitivity to source documents |
| Embeddings/vectors | Numeric representations of chunks | Can leak semantic information; treat as derived sensitive data |
| Prompts and responses | User questions, retrieved context, model answers | May contain source data and user intent |
| Credentials/secrets | LLM keys, connector OAuth tokens, SMTP creds, DB passwords | High-impact secrets |
| User/admin metadata | Accounts, roles, API keys, tenant IDs, audit events | Identity and access data |
| Logs/traces | App logs, request logs, Langfuse/OTel traces, errors | May accidentally include prompts, chunks, credentials, or file names |
| Backups | DB, object store, search index snapshots | Same sensitivity as production data |

## Storage locations

| Store | Default/self-hosted component | Data stored | Production control |
|---|---|---|---|
| Metadata DB | MySQL | Users, tenants, configs, API keys, document metadata, task state | Private network, encryption at rest, backups, restricted admin access |
| Object storage | MinIO or S3-compatible storage | Raw uploaded files, parsed artifacts, possibly generated assets | Private endpoint, bucket policy, encryption, access logs |
| Vector/search index | Elasticsearch, OpenSearch, Infinity, OceanBase, or SeekDB | Chunks, embeddings, text indexes, metadata | Private endpoint, encryption, snapshot/restore policy |
| Cache/queue | Redis/Valkey | Queues, session/task cache, transient data | Private network, auth, no public exposure |
| Logs | Local volume and centralized logging | Application events and errors | Redaction, access control, retention limits |
| Optional observability | Langfuse (per-tenant, `tenant_langfuse` DB table) | Model traces, prompts, responses, timing, model metadata | Enable-able by any tenant admin at runtime (no config gate); control at network layer; internal-only and approved. OTLP is Go-components-only and a no-op unless an exporter is configured. |

## Expected outbound data flows

| Destination | Data that may leave | Required approval |
|---|---|---|
| LLM provider or enterprise gateway | User prompt, system instructions, selected retrieved chunks, chat history needed for context | Required before use |
| Embedding provider | Document chunks and queries | Required unless local/internal TEI is used |
| Rerank/OCR/image-to-text provider | Queries, chunks, images, page snippets, or extracted text | Required before feature enablement |
| Data connectors (~33 bundled) | OAuth tokens, sync requests, document/file content | Approve each connector/source separately; block the rest (`common/data_source/config.py`) |
| Chat channels (6 bundled, always loaded) | Conversation content, retrieved context | Egress to Discord/Telegram/LINE/Feishu/WeCom/QQ once a bot row exists; keep `chat_channel` table empty and block hosts |
| SMTP relay | Email addresses and email content | Approve if email features enabled |
| OIDC/OAuth identity provider | Auth redirects, tokens, user claims | Approve with enterprise IdP team |
| Langfuse (per-tenant) | Prompt/response traces and metadata if a tenant enables it | Prefer internal only; approve trace fields; enforce at network layer since it is not config-gated |

## Data classes recommended for initial approval

Start with a limited pilot:

- Allowed: internal non-regulated documents approved by data owner.
- Not allowed initially: PCI, PHI, secrets, production credentials, highly confidential M&A/legal documents, export-controlled data, or customer data subject to special contracts.
- Reassess after model-provider, retention, deletion, and audit controls are validated.

## Privacy and data protection controls

- Use an approved LLM gateway/provider with contractual restrictions on training and retention.
- Disable or internally route tracing/observability that captures prompts or completions.
- Disable unneeded connectors and chat channels.
- Restrict who can upload documents, create datasets, configure LLM providers, and create API keys.
- Use least-privilege connector service accounts.
- Avoid broad workspace/global connector scopes during pilot.
- Set file-size, file-type, and malware-scanning policies.
- Treat embeddings as sensitive derived data.
- Redact secrets from logs and error messages.
- Encrypt DB, object store, search index, and backups at rest.
- Require TLS for all user and service-to-service paths that leave a host/private network.

## Retention recommendations

| Data type | Suggested initial retention | Notes |
|---|---:|---|
| Source documents | As required by data owner; default 30-90 days for pilot | Must support deletion requests |
| Parsed chunks/embeddings | Same as source document | Delete derived data when source is deleted |
| Chat history | 30-90 days for pilot | May contain sensitive retrieved context |
| Logs | 30-90 days depending on policy | Redact sensitive content |
| Backups | 30-90 days or enterprise standard | Deletion may persist until backup expiry |
| Connector tokens | Until connector disabled or user revoked | Rotate and revoke on offboarding |

## Deletion requirements

A deletion request should remove or expire:

- Raw source file from object storage.
- Parsed text, images, and artifacts.
- Chunks and embeddings from vector/search index.
- Document metadata from MySQL.
- Related task/cache state where applicable.
- Chat sessions if they contain deleted content and policy requires it.
- Backups according to backup retention policy.

## Questions for data owner / privacy review

- Which data classifications are permitted in the first pilot?
- Which model provider or internal gateway is approved for those data classes?
- Are prompts and responses allowed to be logged for troubleshooting?
- What is the required retention period for documents, embeddings, chat history, and logs?
- Is user consent or data-owner approval required before connector sync?
- Are cross-border transfers possible through selected providers or connectors?
- What is the incident notification path if data is sent to an unapproved endpoint?
