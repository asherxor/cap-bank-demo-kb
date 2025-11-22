# HIPAA Security Rule – High-Level Summary (Bank Demo)

> Note: The bank demo is not a healthcare provider, but some Alto/CAP patterns may be reused in health contexts.  
> This document is included for demonstrations where healthcare/PHI-like scenarios are discussed.

## 1. Overview

The HIPAA Security Rule establishes standards for protecting electronic protected health information (ePHI):

- Administrative safeguards.
- Physical safeguards.
- Technical safeguards.

---

## 2. Relevant Concepts for CAP

Even in non-HIPAA environments, the same patterns apply:

- **Confidentiality:** Ensure only authorized parties access data.
- **Integrity:** Ensure data is not altered improperly.
- **Availability:** Ensure data is accessible when needed.

CAP contributes to **integrity and accountability**:

- Evidence of what code was deployed.
- Attestations about how that code was built and tested.
- Anchors that make tampering with history difficult.

---

## 3. Example Mappings

- **Technical Safeguards (Access Control, Audit Controls)**
  - Use CAP evidence to show:
    - Only authorized builds are deployed.
    - Changes are approved and tracked.

- **Integrity**
  - SBOM + attestations show dependencies and build steps.
  - Anchoring to Canon helps detect tampering with historical records.

- **Security Incident Procedures**
  - Incident response playbooks (e.g., disinformation or outage) parallel HIPAA expectations for incident management.

---

## 4. Why Included in a Bank Demo?

- Demonstrates how CAP and Alto patterns can extend to:
  - Health-affiliated products.
  - Insurance products with health data.
- Shows that **the same evidentiary backbone** can support multiple regulatory frameworks beyond classic financial regulations.
