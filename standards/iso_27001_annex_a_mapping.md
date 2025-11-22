# ISO 27001 – Annex A Mapping (Bank Demo View)

> This is a **lightweight mapping** between ISO 27001 Annex A controls and CAP-enabled practices in the bank demo.

## 1. Context

The bank’s ISMS is assumed to align with ISO 27001.  
CAP is used to **collect and anchor evidence** that many Annex A controls are being applied in practice.

---

## 2. Selected Annex A Controls & CAP Support

### A.5 – Information Security Policies

- **Control:** A.5.1 Information security policy.
- **CAP Support:**
  - Policies stored in `policies/` and ingested as `policy_documents` via CAP.
  - Org Agents can reference these documents in analyses.

### A.12 – Operations Security

- **Control:** A.12.1 Operational procedures and responsibilities.
- **CAP Support:**
  - KB playbooks in `kb/` (incident, outage, disinformation).
  - Evidence of procedures being followed:
    - Agent runs promoted to Evidence/Attestations.

- **Control:** A.12.4 Logging and monitoring.
- **CAP Support:**
  - CAP records evidence of:
    - Deployments.
    - SBOMs.
    - Attestation and anchor events.

### A.14 – System Acquisition, Development and Maintenance

- **Control:** A.14.2 Security in development and support processes.
- **CAP Support:**
  - SBOM ingest (`/v1/sbom/ingest`) per build.
  - Attestations for secure pipeline steps.
  - Anchored attestations provide tamper-evident history.

### A.16 – Information Security Incident Management

- **Control:** A.16.1 Management of information security incidents.
- **CAP Support:**
  - Incident-related evidence and attestations stored centrally.
  - Disinformation and outage playbooks reference CAP as a primary evidence store.

---

## 3. Using This Mapping in Practice

- During audits or tabletop exercises:
  - Show how **CAP records** correspond to Annex A controls.
- For each major incident:
  - Ensure final reports and Org Agent Evidence mention:
    - Which Annex A controls were exercised.
    - Where CAP holds the evidence.

This is not a complete ISO 27001 implementation, but a **practical bridge** between CAP’s technical capabilities and governance language.
