# RAGFlow Approval Packet — Reviewer Handoff Summary

## 1. Problem and context

The task was to prepare RAGFlow for an internal architecture/security approval review.

The concern came from a prior comparison: another project was rejected because the architect felt it was not clearly mapped out, looked shoddy, and did not provide enough confidence that there were no hidden egress behaviors. OpenCode was later identified as an example of a project that was approved because its threat model and expected risky behaviors were more explicit.

Based on that context, the goal for RAGFlow was to create a clear review packet that explains:

- What RAGFlow is and how it would be deployed.
- What components exist in the architecture.
- What data flows through the system.
- What outbound egress is expected or should be blocked.
- What deployment hardening is required.
- What risks, privacy issues, and supply-chain concerns should be reviewed before approval.

The working assumption is that RAGFlow is legitimate open-source software, but it should be reviewed as a self-hosted AI infrastructure platform rather than a simple local CLI tool.

## 2. How the task was approached and what was created

I reviewed the local RAGFlow repository and focused on approval-relevant areas: license, README, Docker Compose files, service configuration, dependency manifests, external provider configuration, sandbox/code-execution paths, observability paths, and default exposed ports.

I then created an approval packet in:

`/Users/ankitsingh/Documents/deep-wiki/ragflow-artifacts`

Before this handoff file, I created **8 approval artifacts**:

1. `00_approval_packet_index.md`
2. `01_architecture_data_flow.md`
3. `02_egress_allowlist.md`
4. `03_deployment_hardening_checklist.md`
5. `04_threat_model_risk_register.md`
6. `05_data_handling_privacy.md`
7. `06_sbom_dependency_license_review.md`
8. `07_operational_runbook.md`

This handoff summary is the **9th file** in the folder.

### Update: code-grounded verification pass

After the initial draft, the full packet was **reviewed and corrected against the source code** at commit `065797b047573637c09af771055c723c8b00ecd9`. Every factual claim (ports, default credentials, sandbox configuration, egress endpoints, connectors, chat channels, observability, build/supply-chain hosts, and the `SECURITY.md`/pickle issue) was checked against repository files and updated for precision. A new artifact, **`09_reviewer_verification_log.md`**, records the methodology, the file:line evidence, and the corrections applied — bringing the packet to **10 files (`00`–`09`)**.

The most consequential corrections were: the shipped defaults are *not* safe (default admin `admin@ragflow.io` / `admin`, reused `infini_rag_flow` store passwords, admin server enabled and data-tier ports published to `0.0.0.0`, open registration); chat channels are an always-loaded subsystem rather than "blocked by default"; observability (Langfuse) is per-tenant DB-configured at runtime; and the `SECURITY.md` pickle write-up corresponds to a real, still-present (though currently latent) deserialization RCE now tracked as R21.

The packet was structured to make the review easier for an architect/security reviewer:

- The architecture file includes Mermaid diagrams and trust boundaries.
- The egress allowlist separates runtime egress from build/CI egress.
- The hardening checklist turns risks into concrete deployment controls.
- The threat model lists the main risks and mitigation status.
- The data-handling file explains where documents, chunks, embeddings, prompts, logs, and secrets may live.
- The SBOM file explains what dependency/license/security scans still need to be generated.
- The runbook gives operational controls for deployment, backup, restore, patching, rotation, and incidents.

## 3. What the reviewer should focus on

The reviewer should start with these files, in this order:

1. `00_approval_packet_index.md`  
   Review the executive position and recommended approval conditions.

2. `01_architecture_data_flow.md`  
   Confirm whether the proposed architecture matches the intended deployment model. Pay special attention to internal-only services, trust boundaries, and whether the sandbox/code-executor is disabled.

3. `02_egress_allowlist.md`  
   This is the most important security review artifact. Replace every `TBD` with the actual approved endpoint, or explicitly mark the category as not approved. Confirm runtime egress is default-deny.

4. `03_deployment_hardening_checklist.md`  
   Use this as the go/no-go checklist before pilot or production. The key items are default credentials, exposed ports, secrets management, ingress/TLS, and disabling unused connectors.

5. `04_threat_model_risk_register.md`  
   Review the risk ratings and decide which risks are blockers versus accepted residual risks.

6. `05_data_handling_privacy.md`  
   Confirm which data classifications are allowed in RAGFlow and whether prompts/chunks can leave to the chosen model provider.

7. `06_sbom_dependency_license_review.md`  
   This file is a plan, not a completed scan. The reviewer should require actual SBOM, vulnerability, image, and license scan outputs before production approval.

8. `07_operational_runbook.md`  
   Review once a pilot or production owner is identified. Fill in owners, backup/restore details, patch cadence, and incident-response contacts.

9. `09_reviewer_verification_log.md`  
   Read alongside the others — it shows exactly which claims were confirmed, corrected, or remain `TBD`, each with repository file:line evidence, so the reviewer can audit the packet against the code rather than taking it on trust.

Key open decisions for the reviewer:

- Which LLM gateway or provider is approved?
- Which embedding/reranking/OCR path is approved?
- Which connectors are actually needed?
- Is sandbox/code execution required, or can it stay disabled?
- Which data classes are allowed during pilot?
- What retention/deletion rules apply to documents, embeddings, chat history, logs, and backups?
- Who owns SBOM generation and vulnerability/license triage?

Recommended approval posture: approve only for a controlled pilot after the egress allowlist, deployment hardening checklist, and data-handling decisions are completed.
