# Architecture Overview – Core Banking (Demo View)

> Note: This is a simplified architecture for demo/tabletop purposes, not a full production blueprint.

## 1. Role in the CAP Demo

The core banking system is treated as the **system of record** for:

- Customer accounts.
- Balances.
- Transactions.
- Ledger postings.

In the CAP demo:

- Mobile banking and other channels are **clients** of core banking.
- CAP evidence and attestations help prove:
  - What version of integration code is running.
  - Whether changes could have impacted balances.

---

## 2. Logical Components

- **Core Banking Engine**
  - Account master data.
  - Transaction ledger.
  - Product definitions (accounts, loans, cards).

- **Integration Layer**
  - ESB / API layer providing services to:
    - Mobile banking.
    - Online banking.
    - Branch/ATM systems.

- **Reconciliation & Reporting**
  - Batch jobs.
  - Regulatory reports.
  - Daily and intraday reconciliation.

---

## 3. Security & Control Hooks

- All access to core banking from channels is:
  - Authenticated and authorized.
  - Logged and monitored.

- Change control around:
  - Integration APIs.
  - Customer-visible behavior.
  Is governed by:
  - `policies/change_management_policy.md`
  - NIST CSF & ISO 27001 control references.

In the CAP demo:

- Integration components have SBOMs and attestations.
- Any **unapproved change** would appear as:
  - Missing SBOM.
  - Failed or missing attestation.
  - Lack of Canon anchor.

---

## 4. Dependencies for CAP

For CAP to be effective:

- Integration services must:
  - Participate in SBOM generation.
  - Emit attestations on deploy.
- Incident tickets referencing “customer balances” **must** be linked to:
  - Relevant services (subjects) in CAP.
  - Evidence from logs, reconciliations, and Org Agent analyses.

---

## 5. Typical Demo Questions CAP Helps Answer

1. “Could the last deployment to the mobile integration layer have corrupted balances?”
2. “Do we have any evidence of unauthorized code being deployed?”
3. “Can we show a regulator a **signed and anchored** record of what was running at the time of the incident?”

The architecture overview here is used by:
- CAP users during investigations.
- Org Agents as part of their context when reasoning about core-related incidents.
