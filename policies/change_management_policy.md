# Change Management Policy – Bank Demo

## 1. Purpose

Ensure that all changes to production systems:

- Are controlled and authorized.
- Are **visible and auditable** via CAP.
- Do not introduce unacceptable risk to customers or the bank.

---

## 2. Scope

- Applies to:
  - Application code (mobile backend, APIs, web).
  - Infrastructure-as-code changes.
  - Configuration changes that affect customer-facing behavior.
- Environments:
  - Staging.
  - Production.

---

## 3. Change Types

- **Standard Changes**
  - Pre-approved, low-risk, routine changes.
  - Documented procedures and rollbacks.
- **Normal Changes**
  - Requires full review and approval.
- **Emergency Changes**
  - Fast-tracked changes to fix SEV-1 incidents.
  - Must be documented and retrospectively reviewed.

---

## 4. Required Controls (Linked to CAP)

For any change reaching production:

1. **SBOM**
   - Build pipeline MUST generate an SBOM for the release.
   - SBOM ingested into CAP via `/v1/sbom/ingest`.
2. **Attestation**
   - An attestation must be created and verified in CAP:
     - Includes build metadata (commit, branch, build system).
     - Includes security checks (tests, scans).
3. **Anchoring**
   - For critical services (e.g., mobile banking, core integration):
     - Attestation MUST be anchored via CAP’s anchor provider (`canon_local` in demo).
4. **Approval**
   - Change tickets must reference:
     - CAP Subject ID.
     - Evidence/Attestation IDs where applicable.

---

## 5. Emergency Changes

- Allowed only under SEV-1 / SEV-2 incident conditions.
- Minimum requirements:
  - SBOM/Attestation may be generated immediately after the change if necessary.
  - Post-change Org Agent run:
    - Reviews the change.
    - Produces Evidence/Attestation for audit trail.
- A post-incident review is mandatory.

---

## 6. Non-Compliance

- Deployments without:
  - SBOM,
  - Attestation, or
  - Required approvals
  are considered **policy violations** and must be:

  - Logged as an incident.
  - Investigated.
  - Reported to risk governance as needed.

---

## 7. Relationship to Standards

This policy supports:

- NIST CSF 2.0:
  - PROTECT, DETECT, RESPOND functions.
- ISO 27001:
  - Control A.12 (operations security).
  - A.14 (system acquisition, development, and maintenance).

CAP provides the **evidence substrate** to demonstrate compliance.
