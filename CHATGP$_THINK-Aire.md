Phase 1 — Research & Requirements
1) Domain & Regulatory Deep Dive
1.1 What are the NERC standards?

NERC publishes ~100 enforceable Reliability Standards across families (BAL, CIP, COM, EOP, FAC, INT, IRO, MOD, PER, PRC, TOP, TPL, VAR). Rather than dump 100+ line items here, use NERC’s canonical index (kept current) as the system of record, and we’ll ingest this into the product’s compliance knowledge base. 
NERC

Notable 2024–2025 development: NERC’s new CIP-015-1 (Internal Network Security Monitoring / INSM) is now approved at FERC (Order No. 907 on June 26, 2025), with a directive to extend scope to EACMS/PACS outside the ESP; a rule synopsis posted in the Federal Register on July 2, 2025 clarifies intent. Standards revisions are active through NERC’s 2025–2027 plan. 
Federal Energy Regulatory Commission
+2
Federal Register
+2

1.2 Top 10 “critical & hardest to manage” standards (by operational burden, audit depth, and penalty exposure)

CIP-002 — BES Cyber System identification & impact categorization (foundation that gates all other CIP scope). 
certrec.com

CIP-003 — Cyber security policies for low-impact assets (broad footprint; lots of procedural evidence). 
NERC

CIP-004 — Personnel risk assessments, training, access authorization (people/process heavy). 
certrec.com

CIP-005 — Electronic Security Perimeters & access points (architecture, DMZ, inbound/outbound rules). 
NERC

CIP-006 — Physical security for BES cyber systems & control centers (PACS logs, controls, testing). 
White & Case

CIP-007 — System security management (patching, logging, services, accounts; very evidence-intensive). 
NERC

CIP-008 — Incident reporting & response planning (table-tops, lessons learned, timing). 
NERC

CIP-009 — Recovery plans for BES cyber systems (backups, exercises, restoration evidence). 
NERC

CIP-010 — Configuration change mgmt & vulnerability assessments (baselines, drift, approvals). 
certrec.com

CIP-013 — Supply chain risk management (third-party controls, procurement/contract evidence). 
NERC

Also high-impact O&P standards: PRC-005 (Protection System Maintenance & Testing), FAC-003 (Vegetation Mgmt), FAC-008 (Facility Ratings), TOP-001 / TPL-001 (Operations/Planning). PRC-005 is a frequent non-compliance source due to schedule tracking/interval math. 
NERC
+1

1.3 Current manual compliance workflow (today)

Scope & applicability: determine standard→requirement→part applicability by Registration & BES impact levels; create an RSAW per standard. 
NERC

Evidence collection: policy docs, screenshots, dated spreadsheet outputs, system & security logs, config exports, tickets, training rosters, access reviews, and reports—bundled to the RSAW narrative. 
NERC
+1

Audit prep “fire drill”: comply with Regional Entity data calls; finalize RSAWs; stage evidence packages; perform Internal Controls Evaluation (ICE) to tune scope and testing plans. 
NERC
+2
NERC
+2

On audit: auditors follow CMEP Manual steps; sample evidence; assess internal controls; issue AOCs/PNCs; track mitigation. 
NERC

1.4 Common failure points, fines & risks

Frequent pitfalls: bad scoping (CIP-002), incomplete patching or logging (CIP-007), weak access lifecycle (CIP-004/005), missing baseline & change approvals (CIP-010), weak RSAW narratives/evidence mapping; PRC-005 maintenance interval errors. 
NERC
+2
certrec.com
+2

Penalties: up to $1M per day per violation under FPA §215; multi-million-dollar CIP cases are public record (e.g., $10M 2019 action). 
Federal Energy Regulatory Commission
+2
Federal Energy Regulatory Commission
+2

2) User Personas & Stakeholders

Compliance Manager (primary buyer/user): needs a live audit-readiness dashboard: coverage by standard/requirement/asset class; upcoming cadence tasks (patches, reviews); evidence package completeness & freshness; RSAW status; gaps vs. IRA/ICE findings; mitigation plan tracking.

Grid/System Operator (OT operations): minimal friction. Receives copilot nudges only when action is needed (e.g., “apply approved config baseline v12 on RTAC-A”); in-HMI sidebar that’s read-only during active ops; offline batch tasks aligned with maintenance windows.

IT/OT Security Analyst: hunts misconfig, failed patches, anomalous east-west comms (INSM), unauthorized ports/services; wants guided remediation runbooks and automatic “evidence-of-fix” capture.

External Auditor (read-only): time-boxed portal exposing frozen evidence sets mapped to RSAWs + cryptographic attestations, sampling navigation, and Q&A threads—no production access.

3) Core Functionality & Feature Set
3.1 Automated Evidence Collection

Inbound sources (MVP priority):

SIEM: Splunk, Microsoft Sentinel.

Endpoint/Server: Windows/Linux logs; EDR (Defender for Endpoint, CrowdStrike, etc.).

Network & Security: firewalls (Fortinet, Cisco), switches/routers (NetOps logs), NAC.

OT/ICS: historian (AVEVA PI System) for operational state snapshots; substation/IED telemetry via vendor APIs (SEL), and syslog/TAP for INSM. 
Aveva
+1

Identity: AD/Entra ID for user/role evidence.

Physical Access (PACS): door controller logs, badge events (mapped to CIP-006/004).

ITSM: ServiceNow/Jira ticketing to correlate approvals and change records.

How mapping works: every record ingested is normalized to a control ontology: {Standard → Requirement → Part}. Example: a firewall ACL change with approved CAB ticket + config diff is auto-tagged to CIP-010 R1 (baselines) & R2 (change mgmt), plus KPI updates (MTTR, %approved). We leverage requirement texts/tables (e.g., CIP-007-6 measures) to drive validators. 
NERC

3.2 AI-Powered Anomaly Detection (Ops + Compliance)

What we detect (initial models):

Config drift vs. golden baselines (devices, OS hardening, service enablement) → CIP-010.

Patch posture anomalies (missed cycles, at-risk CVEs) → CIP-007.

Access anomalies (privilege creep, shared accounts, orphaned badges, off-hours access) → CIP-004/005/006/007.

East-west traffic anomalies inside ESPs per CIP-015 INSM (new device, new service, new flow, beaconing). 
Federal Register

Alert → remediation flow:
Alert (with mapped controls + confidence) → create ticket in ServiceNow/Jira with copilot playbook (affected assets, blast radius, “why this violates R#,” step-by-step fix, maintenance window hints) → after closure, system auto-collects evidence of fix (logs, diffs, screenshots) and updates RSAW package status.

3.3 Audit Prep & Copilot

Auto-generated reports: Reliability Standard Audit Worksheets (RSAWs), sampling manifests, ICE summaries, mitigation status, and auditor-shareable evidence bundles. 
NERC
+1

Copilot Q&A examples:

“Show all evidence for CIP-005 R3 (last quarter) with sampling IDs.”

“Which medium-impact assets are non-compliant on CIP-007 R2 patching this month?”

“Export my CIP-010 baseline drift exceptions since 2025-07-01 with approvals.”

“Build the RSAW narrative for CIP-003-10, citing attached procedures and evidence list.” 
NERC

4) Data & AI/ML Strategy

Ingestion from secure/air-gapped OT:

On-prem secure collector(s) in the ESP with SPAN/TAPs; push via data diode/unidirectional gateway to DMZ for relay to cloud VPC, or retain fully on-prem. NIST 800-82 recognizes these ICS patterns; NERC’s INSM technical docs even depict data diodes as an option between collection and analysis. 
NIST Publications
+1

Alternative: one-way file drops (WORM/S3-compatible) or secure agent to a customer VPC (single-tenant).

Models:

RAG Copilot: LLM with a curated corpus (all NERC standards texts, RSAWs, CMEP/ICE guidance, local policies/procedures) and retrieval filters by standard/req/asset. 
NERC
+1

Anomaly detection: time-series/statistical (Isolation Forest/Autoencoders for flows), config-graph diffs, and rules for explicit requirement checks.

Classifiers: map unstructured artifacts (operator logs, screenshots) to control ontology (e.g., “CIP-007 R4 security event monitoring”). 
NERC

Vector DB: Qdrant or pgvector (customer-managed, encryptable, air-gap friendly). We choose one per deployment profile (see Phase 2).

5) Technical & Security Architecture

Deployment model (recommended default): Hybrid — on-prem Collectors + Evidence Vault (optional), with single-tenant VPC for AI/analytics, reporting, and auditor portal. Offer pure single-tenant cloud or fully on-prem appliance where mandated.

Platform security (our SaaS): SOC 2 Type II, ISO 27001 roadmap; end-to-end encryption (TLS 1.3; server-side AES-256 with customer-managed keys), strict RBAC/ABAC, JIT admin, immutable evidence store (hash-chaining + WORM), full audit logs, private networking (AWS/Azure PrivateLink), and no cross-tenant data. Penalty exposure justifies defensible controls aligned with CMEP expectations. 
Federal Energy Regulatory Commission

6) Integration Ecosystem (MVP must-haves)

Sources: Splunk, Microsoft Sentinel; Windows Event Forwarding/Linux syslog; Fortinet/Cisco; SEL (OT ecosystem) and AVEVA PI System for historian context; PACS systems; AD/Entra ID. 
selinc.com
+1

Sinks: ServiceNow, Jira, PagerDuty for alerts/incidents & remediation.

Identity: AD/Entra ID (SCIM, SAML/OIDC) for provisioning/SSO and user evidence.

Phase 2 — Solution Architecture & Workflows
1) High-Level Solution Architecture (hybrid)
[OT/ESP Network]                    [Utility DMZ]                     [Single-tenant Cloud VPC]
 ┌───────────────────────┐          ┌─────────────────────┐           ┌──────────────────────────────┐
 │ OT Assets/IEDs/RTACs │──SPAN/TAP→│ On-prem Collector   │─(one-way)→│ Ingestion API Gateways        │
 │ (SEL, relays, PLCs)  │           │  (packet/flow,      │  diode    │ (private endpoints)          │
 ├───────────────────────┤           │  logs, configs)     │──────────→│ Streaming Bus (Kafka/NATS)   │
 │ HMI/SCADA servers    │           └─────────────────────┘           │                              │
 │ Historians (PI)      │──API→                                           ┌────────────────────────┐
 │ PACS controllers     │──syslog→    (optional file courier)             │ Secure Data Lake       │
 └───────────────────────┘                                                │  - Relational (Postgres│
                                                                           │  - Time-series (TSDB)  │
          [IT Network]                                                     │  - Object/WORM store   │
 ┌───────────────────────┐                                                │  - Vector DB (Qdrant)  │
 │ Servers/EDR/AD        │──agent/syslog→                                 └────────────────────────┘
 │ SIEM (Splunk/Sentinel)│──API→             ┌────────────────────────┐   ┌────────────────────────┐
 └───────────────────────┘                   │ AI/ML Core             │   │ Compliance Apps        │
                                             │ - INSM models (CIP-015)│   │ - RSAW Builder         │
                                             │ - Config/Patch models  │   │ - Evidence Catalog     │
                                             │ - RAG Copilot (LLM +   │   │ - Auditor Portal       │
                                             │   standards corpus)    │   │ - Dashboards/APIs      │
                                             └────────────────────────┘   └────────────────────────┘


Notes: INSM/east-west analytics aligned to CIP-015; optional data diode per NERC technical rationale; all evidence immutably stored and mapped to standards. 
NERC

2) Core User Workflow (Sequence: “Anomaly → Remediation → Evidence”)

Network Device emits config change; Collector sees drift vs. baseline.

AI Detector (CIP-010 rules + anomaly model) flags non-approved change; maps to CIP-010 R1/R2.

Platform opens ServiceNow ticket with copilot playbook (root cause, impacted assets, change window).

Compliance Manager gets dashboard alert; Operator receives guided steps (no-disruption mode if in outage window).

Operator applies fix within change window.

Platform auto-captures “evidence of fix” (device diff, logs, CAB approval, timestamps), updates RSAW status, recalculates compliance KPIs, and freezes an auditor bundle for that requirement.

3) Technology Stack Recommendation

Frontend: React + TypeScript (Next.js), Tailwind/shadcn UI, component library for data-dense tables; OIDC SSO; offline “evidence cart” builder.

Backend (APIs): Python FastAPI microservices; gRPC between services; policy engine (OPA) for ABAC.

Data:

Relational: PostgreSQL (config, users, mappings, tickets).

Time-series: TimescaleDB (or InfluxDB if preferred) for metrics/flows.

Search/logs: OpenSearch for log search across evidence.

Object/WORM: S3-compatible with bucket-level legal hold & retention.

Vector DB: Qdrant or pgvector (per deployment constraints).

AI/ML: Python (scikit-learn, PyTorch), RAG with LangChain/LlamaIndex; prompt-guardrails with retrieval filters locked to requirement/part; model registry (MLflow).

Data ingestion: Kafka (or NATS JetStream) for durable streams; connectors for Splunk/Sentinel/PI/EDR.

Cloud/Infra: Kubernetes (EKS/AKS), Terraform/IaC, PrivateLink, KMS with customer-managed keys, HashiCorp Vault, Falco + Kyverno policies, SIEM export.

Security: mTLS, workload identity, immutable audit logs, SAST/DAST/SBOM, CIS benchmarks; tamper-evident hashing for evidence chains.

4) MVP Features & Prioritization

Must-Have (MVP, 4–6 months):

Standards engine (read-only corpus) + control ontology; RSAW builder for CIP-003/004/005/007/010. 
NERC

Integrations: 2 SIEMs (Splunk, Sentinel); AD/Entra; Fortinet (or Cisco) firewall; Windows/Linux logs; ServiceNow.

Hybrid deployment (collector + single-tenant VPC) with immutable Evidence Catalog.

Copilot (RAG) constrained Q&A with audit-safe citations back to artifacts/requirements.

INSM lite: east-west flow anomaly detection inside ESP (packet metadata, new device/service/flow baselines) to align early with CIP-015. 
Federal Register

Should-Have (V2):

Historian (AVEVA PI) integration for operational context; PACS feeds; guided table-top modules (CIP-008/009). 
Aveva

Jira & PagerDuty sinks; evidence freeze & auditor portal with time-boxed access; ICE helper.

Could-Have (Future):

Auto-draft mitigation plans; control simulation (“what if we change network segment X?”); contract parsing for supply-chain (CIP-013) with clause coverage scoring; DER/IBR overlays tied to Order 901 projects. 
NERC

5) Go-to-Market & Business Model

Packaging:

Core Copilot + Evidence (CIP-003/004/005/007/010 bundle).

INSM add-on (CIP-015 alignment) for medium/high impact ESPs.

O&P add-ons (PRC-005, FAC-003/008) with specialized evidence templates.

Pricing: hybrid subscription by BES asset class/impact tier + data volume + module pack (CIP bundle(s)); on-prem appliance priced by collector nodes. Optional white-glove onboarding/audit-season surge.

#1 CISO Objection: “No cloud egress from ESP / data sovereignty.”
Answer: Offer three deployment tiers:

Fully on-prem (all analytics local).

Hybrid with data diodes (metadata only to VPC; raw artifacts remain on-prem; CMKs).

Single-tenant VPC in customer’s cloud account with private links. (Backed by SOC 2/ISO 27001, immutable evidence, customer-managed keys, and auditable controls.) 
NIST Publications
+1

Appendix — Why now (regulatory tailwinds)

CIP-015-1 (INSM) is now approved: FERC Order No. 907 (June 26, 2025) with scope-expansion directives; Federal Register details (July 2, 2025). Your platform can operationalize INSM and auto-map detections to RSAWs

PRD v0.1 — “AI Compliance Copilot for NERC”
0) Summary

An AI-native platform that automates evidence collection, anomaly detection, and “always audit-ready” preparation across priority NERC standards (initially CIP-003/004/005/007/010 + INSM/CIP-015 alignment). Hybrid deployment: on-prem collectors + single-tenant cloud analytics, with fully on-prem option.

1) Goals & Non-Goals

Goals (MVP)

Map ingested telemetry/artifacts to specific NERC requirements/parts.

Detect high-value anomalies (config drift, patching misses, access anomalies, east-west flow anomalies for INSM).

Generate RSAWs and freeze cryptographically attested evidence bundles.

Provide a constrained, citation-backed Copilot for queries and guided remediation.

Integrate with Splunk + Microsoft Sentinel (sources) and ServiceNow (sink), AD/Entra for SSO + identity evidence.

Non-Goals (MVP)

Full coverage of every NERC family.

Automated patch deployment or config push to OT (read/advise only).

Cross-utility multi-tenant SaaS.

2) KPI / Success Metrics

≥80% automated evidence coverage for targeted controls (CIP-003/004/005/007/010).

≤15 min median time from anomaly detection → ticket creation with mapped requirement.

≥90% of Copilot answers include at least one linked artifact + requirement citation.

Audit cycle prep time reduced ≥50% vs. baseline (measured during design-partner audits).

Zero production data egress from ESPs where “fully on-prem” tier is chosen.

3) User Stories (MVP)

Compliance Manager

As a CM, I see live “audit-readiness” by standard/requirement/asset class and due tasks (patch reviews, access recerts).

As a CM, I can generate an RSAW draft for CIP-007 with evidence list auto-attached and gaps highlighted.

IT/OT Security Analyst

As an Analyst, I receive an alert when a firewall rule changes without CAB approval (mapped to CIP-010 R1/R2) and get a guided playbook.

As an Analyst, I can baseline allowed services and detect new east-west flows within the ESP (INSM).

Grid/System Operator

As an Operator, I get quiet, context-aware nudges (during maintenance windows) with steps to fix a drifted baseline without disrupting operations.

External Auditor

As an Auditor, I receive time-boxed portal access to frozen evidence bundles with RSAW narratives and sampling indexes—no production access.

4) Scope (MVP Features)

Evidence Collection & Catalog

On-prem Collectors (ESP, IT) with log/flow/config ingestion.

Evidence normalization + immutable storage (hash-chain, WORM).

Mapping engine: artifact → {Standard→Req→Part}.

Anomaly Detection

Config drift vs. golden baseline (CIP-010).

Patch posture anomalies (CIP-007).

Access anomalies (accounts, badges, off-hours) (CIP-004/005/006/007).

East-west flow anomalies inside ESP (INSM/CIP-015 alignment).

Audit Prep & Copilot

RSAW builder with templated narratives and auto-attach of artifacts.

Copilot with RAG over standards corpus + utility policies; strict retrieval filters and artifact citations.

Evidence Freeze & Auditor Portal.

Integrations

Sources: Splunk, Microsoft Sentinel, AD/Entra, Windows/Linux syslog, Fortinet (or Cisco) firewalls.

Sinks: ServiceNow (incidents/change), PagerDuty (notifications).

Identity: SAML/OIDC SSO with SCIM.

5) Acceptance Criteria (MVP Excerpts)

Mapping engine achieves ≥95% precision on deterministic mappings (e.g., a CAB-approved config diff → CIP-010 R2).

Anomaly pipeline generates a ServiceNow ticket within 2 minutes (P50) of drift detection, includes mapped requirement, impacted assets, and copilot playbook URL.

RSAW export produces a single ZIP/PDF package with content indexes, artifact hashes, timestamps, and signer cert.

Copilot answers never return free-text claims about compliance status without a linked artifact/requirement.

6) Non-Functional Requirements

Security: TLS 1.3, mTLS service-to-service, AES-256 at rest, CMKs supported; RBAC + ABAC; all admin actions JIT + audited.

Privacy/Residency: three deployment tiers (fully on-prem; hybrid with metadata to VPC; single-tenant in customer VPC).

Reliability: SLOs—99.9% control plane; data loss ≤ 0.1% on backpressure (exactly-once processing where feasible).

Auditability: immutable audit log of all mappings, transformations, and RSAW changes.

Systemic Control Ontology (V0)
1) Entities & Canonical IDs

Standard: {FAMILY}-{###}-{REV} (e.g., CIP-007-6)

Requirement: STD:R# (e.g., CIP-010-3:R1)

Part: STD:R#:P#.# (e.g., CIP-010-3:R1:P1.1)

EvidenceType (examples): CONFIG_DIFF, LOG_EVENT, TICKET, SCREENSHOT, TRAINING_ROSTER, BADGE_EVENT, BASELINE_FILE, POLICY_DOC

AssetClass: ControlCenter, Substation, ESP_Device, EACMS, PACS, HMI, Historian, Server, Workstation

ImpactLevel: Low, Medium, High

2) Mapping Rules (examples)

Firewall config change → CONFIG_DIFF + TICKET with CAB approval → CIP-010:R2 (+ R1 if baseline updated).

Missed patch window (Windows server >30 days) → LOG_EVENT (WSUS/SCCM) + ASSET → CIP-007:R2.

New east-west service in ESP not in allowlist → FLOW_EVENT → CIP-015:R? (INSM) + CIP-005:R1/R2 (ESPs & access points).

Badge access outside normal hours to control room without work order → BADGE_EVENT + WORK_ORDER → CIP-006:R? and possibly CIP-004.

3) Validation & Scoring

Freshness: each requirement has an evidence freshness window (e.g., CIP-007:R2 monthly).

Completeness: % of required EvidenceTypes present per requirement.

Confidence: rules > ML > manual assertions (weighted).

Data Model (ERD-in-text)

Core relational (PostgreSQL / TimescaleDB)

standards(id, family, number, revision, title, text_ref)

requirements(id, standard_id, r_num, title, measures_ref)

requirement_parts(id, requirement_id, part_num, description)

assets(id, name, class, impact_level, location, owner)

evidence(id, type, hash, uri, collected_at, source_id, asset_id, size_bytes, chain_prev_hash, chain_sig)

mappings(id, evidence_id, req_part_id, confidence, rule_id)

events(id, kind, asset_id, occurred_at, payload_jsonb) // drift, patch, access, flow

tickets(id, external_id, system, status, url, created_at, req_part_id)

rsaw_packages(id, standard_id, version, status, frozen_at, zip_uri, signer_cert_id)

auditor_access(id, org_id, rsaw_package_id, start_at, end_at, role)

users(id, email, display_name, org_id)

roles(id, name) + user_roles(user_id, role_id)

integrations(id, type, config_jsonb, status)

collectors(id, site, network_zone, version, last_seen_at)

Vector store (Qdrant/pgvector)

kb_chunks(id, doc_id, std_or_policy_ref, embedding, chunk_text, metadata_jsonb)

artifact_embeddings(id, evidence_id, embedding, labels[])

Object/WORM

Raw artifacts (PDFs, config dumps, screenshots), RSAW exports, signed manifests.

Time-series

metric_series (e.g., patch posture %, baseline drift count, flow cardinality).

Hash-chain for evidence immutability

Each evidence.chain_prev_hash + current record hash → chain_sig (sign with org key).

API Contracts (V0)
External REST (UI/Integrator)

Auth: OIDC (SAML optional), OAuth2 bearer; SCIM for provisioning.

Evidence

POST /evidence
Body: { type, assetId, collectedAt, source, uri|inline, metadata }
Returns: { evidenceId, hash, chainSig }

GET /evidence/{id}

POST /evidence/{id}/map
Body: { reqPartId, confidence, ruleId }

Mapping & Status

GET /compliance/status?standard=CIP-007-6
→ { requirementParts: [{id, completeness, freshness, score}], gaps: [...] }

POST /rsaw/{standard}/generate
→ { packageId, status, downloadUrl }

POST /tickets
Body: { reqPartId, title, severity, playbookRef, externalSystem }
→ { ticketId, externalId, url }

Copilot

POST /copilot/query
Body: { question, scope: { standards[], assets[], timeRange }, strict:true }
→ { answer, citations:[{artifactId|kbChunkId, reqPartId}] }

Auditor Portal

POST /auditor/grant
Body: { packageId, startAt, endAt, email|federatedId }
→ { portalUrl }

Collector Ingest (secure)

POST /ingest/events (mTLS, allow-list)
Body: Array of { kind, asset, occurredAt, payload }

PUT /ingest/config-baseline/{asset}
Body: { version, files[], signer, approvedTicket }

Webhooks (outbound)

POST https://servicenow/... on new Finding / AnomalyDetected

POST https://pagerduty/... for P1 events

gRPC (internal Microservices)

AnomalyDetector.Detect(Events) → Findings

Mapper.AutoMap(Evidence) → Mapping[]

RAG.Query(QuerySpec) → Answer{citations[]}

OpenAPI note: deliver as openapi.yaml + .proto in source repo.

Detection & Playbook Specs (MVP)
1) Config Drift (CIP-010)

Input: config_snapshot (running), baseline_snapshot (approved), approvedTicketId?

Logic: structured diff → rules (e.g., ACL added, service enabled) → mapping CIP-010:R1 (baseline) or R2 (change).

Output: Finding{severity, reqParts, asset, diffSummary, remediation}

Playbook: “Review diff → if approved, update baseline (CAB ticket link); if not, revert; attach new diff as Evidence.”

2) Patch Posture (CIP-007:R2)

Input: WSUS/SCCM feed or Sentinel query; Timescale window monthly.

Logic: non-applied critical patches past window → Finding.

Playbook: “Open change; schedule maintenance; apply patch; auto-collect logs; re-scan; freeze evidence.”

3) Access Anomalies (CIP-004/005/006/007)

Input: AD events, PACS badge logs.

Logic: privileged account without current training; off-hours control room entry without WO.

Playbook: “Revoke until training; incident note; attach training roster + badge logs.”

4) INSM / East-West (CIP-015 aligned)

Input: flow metadata (collector SPAN/TAP); learned baseline.

Logic: new device/service/flow inside ESP; beaconing to unknown.

Playbook: “Quarantine in IT (advice only in OT); open ticket; capture pcap summary; update allowlist if legitimate.”

RSAW Builder (Template Skeleton)

Header: Standard, version, applicability, registered functions impacted.

Narrative: Control description, implementation overview, roles, tooling.

Measures & Evidence Table:

Requirement Part	Evidence Types	Artifact IDs	Freshness	Notes

Sampling Index: assets, time windows, reviewers.

Exceptions & Mitigations: links to tickets, due dates.

Signature & Freeze: signer DN, timestamp, manifest hash.

UX Flows (Key Screens)

Audit-Readiness Dashboard

Tiles by standard; drill-down by requirement; status bars for Freshness / Completeness / Gaps.

“Export RSAW” and “Freeze Bundle” actions.

Anomaly → Ticket

Finding detail → mapped req parts → “Create ServiceNow ticket” → Copilot playbook → link to evidence timeline.

Evidence Catalog

Facets (standard, requirement, asset, EvidenceType, time).

Chain-of-custody sidebar (hashes, signer, lineage).

Auditor Portal

Read-only RSAW, evidence viewer, Q&A threads; no access to live systems.

Technology Stack (Confirmed)

Frontend: Next.js (React + TS), Tailwind + shadcn/ui, TanStack Table; Auth via OIDC; GraphQL façade (Apollo) over internal REST.

Backend: FastAPI (Python) microservices; gRPC for ML + mapping; OPA (ABAC) sidecar.

Data: PostgreSQL + TimescaleDB; OpenSearch; S3-compatible WORM; Qdrant (vector) or pgvector; HashiCorp Vault; KMS (CMKs).

Ingestion: Kafka (Confluent or MSK) or NATS JetStream; connectors for Splunk, Sentinel, AD, Fortinet/Cisco; file courier for air-gap drops.

ML: PyTorch/sklearn; MLflow registry; LangChain/LlamaIndex for RAG; prompt-guardrails; unit tests with synthetic corpora.

Infra: Kubernetes (EKS/AKS), Terraform, PrivateLink/VNet Peering, Falco/Kyverno, Trivy/SBOM; Loki+Tempo+Prometheus for observability.

Security & Compliance Blueprint

Controls: SOC 2 Type II baseline; ISO 27001 roadmap; CIS Benchmarks (EKS/AKS, Linux, Kubernetes).

Crypto: TLS 1.3; mTLS; AES-256; artifact signing; evidence hash-chains; CMKs and HSM integration optional.

Identity: SSO (SAML/OIDC), SCIM; RBAC roles (OrgAdmin, ComplianceManager, Analyst, Operator, AuditorRO); ABAC attributes (site, impact level).

Data Governance: evidence retention policies; legal holds; per-artifact access control; redaction utilities for sensitive fields.

Routing/Egress: policy engine ensures chosen deployment tier rules; on-prem tier blocks cloud egress.

Backlog & Milestones
Milestone 0 (Weeks 0–3)

Control ontology scaffolding; standards corpus loader.

Collector agent POC (syslog, config snapshot, SPAN metadata).

Data model + OpenAPI/proto v0.1; tenant VPC boilerplate.

Milestone 1 (Weeks 4–8)

Mapping engine (rule-based); Evidence Catalog (immutable store).

Drift detection & patch posture detectors; ServiceNow integration.

RSAW generator v0 (CIP-007, CIP-010); Dashboard v0.

Milestone 2 (Weeks 9–14)

Copilot (RAG) with retrieval filters + artifact citations.

INSM (flow baseline) lite; Auditor Portal (read-only, time-boxed).

AD/Entra + Sentinel connectors; WORM retention policies.

Milestone 3 (Weeks 15–24)

V2 mappings (PACS, badge anomalies); PI Historian context.

Jira/PagerDuty sinks; evidence freeze workflow; ICE helper.

Hardening, SOC 2 readiness; pilot audits with design partners.

Test Plan (High Level)

Unit: mapping rules, hash-chain verification, RSAW export manifest.

Integration: Splunk/Sentinel → Kafka → Detector → ServiceNow ticket E2E in ≤ 2 min P50.

Security: pen test; secrets scanning; policy eval (OPA) for sensitive endpoints.

Reliability: backpressure tests; replay from Kafka with idempotent writes.

Copilot: retrieval precision tests; hallucination guard (answers must carry citations).

Design Partners — Profile & Plan

Ideal mix (3 orgs):

IOU/TO with medium & high impact BES Cyber Systems (active ESPs).

G&T or large muni/co-op with substantial low-impact footprint (CIP-003 reach).

ISO/TO or utility with mature SIEM/ServiceNow stack.

Selection Criteria

Upcoming audit within 6–12 months.

Existing Splunk OR Sentinel + ServiceNow.

Willing to allow non-prod telemetry and redacted artifacts in a sandbox.

Partner Commitments

6-hour/month SME time (Compliance + OT/IT Security).

Access to test network/config snapshots; historical tickets.

Success metrics agreement; reference permission if outcomes met.

What they get

Preferential pricing for 3 years.

On-site audit-season surge support.

Feature influence priority.

Example Data Schemas
Event (Kafka/NATS)
{
  "id": "evt_01HZY...",
  "kind": "CONFIG_DRIFT|PATCH_STATUS|ACCESS|FLOW",
  "assetId": "asset_sub_001",
  "occurredAt": "2025-10-26T12:31:25Z",
  "payload": {...},
  "source": "collector-esp-west",
  "integrity": { "hash": "sha256:...", "sig": "..." }
}

Finding
{
  "id": "find_...",
  "reqParts": ["CIP-010-3:R2", "CIP-010-3:R1"],
  "severity": "High",
  "assetIds": ["fw-rtac-a"],
  "summary": "ACL added without CAB approval",
  "recommendedPlaybook": "pb-config-revert-or-approve",
  "evidenceIds": ["ev_...diff", "ev_...ticket"]
}

RSAW Manifest
{
  "standard": "CIP-007-6",
  "version": "2025.10.26-1",
  "artifacts": [
    {"evidenceId":"ev_123","type":"LOG_EVENT","hash":"sha256:...","path":"s3://..."},
    {"evidenceId":"ev_456","type":"TICKET","hash":"sha256:..."}
  ],
  "signer": {"cn":"AegionCPLT","serial":"..."},
  "frozenAt": "2025-10-26T13:11:02Z",
  "manifestHash": "sha256:..."
}

Pricing & Packaging (Launch Draft)

Core Bundle (per utility + per impact tier + data volume): CIP-003/004/005/007/010 + Evidence Catalog + RSAW + Copilot.

INSM Add-On: INSM analytics (CIP-015 alignment) for medium/high impact ESPs; priced by ESP site count.

On-Prem Appliance: per collector node + annual support; optional HSM integration priced separately.

Primary Objection & Response

Objection: “We cannot send OT data to the cloud.”

Response: Offer fully on-prem analytics tier; or hybrid with one-way metadata replication and CMKs, no raw artifact egress. Provide architectural attestation package (controls, diagrams, pen-test summary) aligned to auditor expectations.
