# Disinformation Playbook – Social Media Campaigns Against the Bank

## 1. Scenario

An organized disinformation campaign targets the bank on social media:

- Claims of **“stolen” balances** after a mobile outage.
- Fake screenshots showing manipulated account balances.
- Fake “leaks” of internal documents or SBOMs.
- Bot amplification and coordinated hashtag campaigns.

This playbook describes how to respond using **CAP evidence and attestations** as a trusted backbone.

---

## 2. Objectives

1. Rapidly determine what actually happened (if anything).
2. Preserve and analyze evidence:
   - Deployments.
   - Changes.
   - Incidents.
3. Maintain **narrative control** based on verifiable facts.
4. Support regulators and partners (e.g., Alto) with **traceable evidence**.

---

## 3. Detection & Triage

### 3.1 Signals

- Social media monitoring flags spike in negative sentiment.
- Multiple customers report seeing the same rumor.
- Alto / external intel partners provide indicators.

### 3.2 Initial Questions

- Is there any **real** incident in progress?
- Did we deploy anything to mobile banking in the last 24–48 hours?
- Are there active incident records in CAP relating to:
  - Mobile banking.
  - Customer balances.
  - Core banking integration.

---

## 4. Using CAP in the Disinformation Response

1. **Check recent deploys for the affected services**:
   - Query CAP for:
     - Latest **SBOMs** and their **attestations**.
     - Anchor status to Canon (via `/v1/attestations/{id}/anchor` & `/v1/anchors/{anchor_id}/verify`).
2. **Launch an Org Agent run** focused on the disinformation event:
   - Agent uses:
     - `kb/incident_playbook_mobile_outage.md`
     - `policies/communication_policy_crisis.md`
     - SBOM + attestation evidence.
   - The run produces a structured assessment and suggested messaging.
3. **Promote the Agent Run to Evidence/Attestation**:
   - Use CAP’s promotion endpoint (Agent Run → Evidence → Attestation).
   - This creates a machine-readable trace of:
     - What was checked.
     - What was found.
     - The recommended action.

---

## 5. External Communications

All public statements must:

- Be grounded in **CAP-backed evidence**.
- Be consistent with:
  - `policies/incident_response_policy.md`
  - `policies/communication_policy_crisis.md`

Example statements:

- “We have verified the integrity of the latest release using our attestation platform. No unauthorized changes were deployed.”
- “System logs and cryptographically anchored attestations confirm that customer balances have not been altered.”

---

## 6. After-Action

After the disinformation wave:

1. Run a **post-incident Org Agent** on:
   - SBOMs and attestations around the time window.
   - Social media data (where available).
2. Promote the run to Evidence/Attestation.
3. Use CAP’s record as the internal **“single source of truth”** for audits and regulators.

This document is used by both the **Bank Comms team** and **Alto / CAP operators** during tabletop exercises.
