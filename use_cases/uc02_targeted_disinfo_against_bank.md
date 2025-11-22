# Use Case UC02 – Targeted Disinformation Campaign Against the Bank

## 1. Scenario Summary

Shortly after a mobile banking outage (UC01), a **targeted disinformation campaign** emerges:

- Viral posts claim:
  - “The bank lost customer funds.”
  - “Hackers changed balances inside the system.”
- Fake screenshots show:
  - Negative balances.
  - Impossible transactions.
- Bot accounts amplify the narrative, tagging journalists and regulators.

The bank and Alto respond using CAP to **separate reality from narrative attack**.

---

## 2. Actors

- **Bank Communications / PR**
- **CISO / Security Leadership**
- **Alto Intelligence** (disinformation & narrative analysis)
- **Regulators / Supervisors**
- **CAP Platform + Canon**

---

## 3. Pre-Conditions

- UC01 (mobile outage) has already occurred and been handled.
- CAP contains:
  - SBOMs + attestations for:
    - Mobile banking.
    - Core banking integration.
  - Anchors for key attestations to Canon.
  - KnowledgeArtifacts and Policies:
    - `kb/disinformation_playbook_social_media.md`
    - `policies/communication_policy_crisis.md`
    - `policies/incident_response_policy.md`

- Org Agents configured:
  - “Disinformation triage agent”.
  - “Regulator briefing agent”.

---

## 4. Trigger

- Alto’s monitoring or bank’s social media team detects:
  - Coordinated posts with the same narrative.
- The narrative explicitly claims:
  - The **system of record** has been altered.
  - The bank is not telling the truth.

---

## 5. Flow with CAP

### Step 1 – Fact Check via CAP

1. Security and operations teams:
   - Confirm there are **no new deployments** touching core balances.
   - Pull:
     - SBOMs and attestations for relevant services.
     - Anchor proofs from Canon.
2. They verify:
   - No unauthorized code or config changes.
   - All attestations related to recent releases validate successfully.

### Step 2 – Disinformation Org Agent

1. A specialized Org Agent is run with context:
   - Disinformation playbook (`kb/disinformation_playbook_social_media.md`).
   - Incident data from UC01.
   - CAP evidence (SBOM, attestations, anchor proofs).
2. The Agent:
   - Produces a structured summary:
     - What rumors are being spread.
     - What the evidence shows.
     - What messaging is safe and accurate.

### Step 3 – Promotion to Evidence & Attestation

- The Agent run is promoted to:
  - Evidence (`ai_assessment`).
  - Attestation (`agent_run`) with links to:
    - KnowledgeArtifacts used.
    - Related incidents and SBOM/attestation records.

This gives the bank a **cryptographically anchored record** of how it assessed the disinformation.

### Step 4 – Communication & Regulator Briefing

- Communications:
  - Use the Agent output + policies to craft public statements.
- For regulators:
  - CAP produces:
    - Evidence/Attestation bundle.
    - Canon anchor receipts.
  - This shows:
    - When code was last changed.
    - What checks were performed.
    - That customer balances remained consistent.

---

## 6. Outcomes

- The bank can say:
  - “Here is the verifiable, anchored record showing that balances were not altered.”
- Alto can:
  - Integrate CAP-based facts into broader narrative analysis and reporting.
- Regulators see:
  - A **repeatable, evidence-backed process**, not ad-hoc PR.

This use case is ideal for:
- Board-level demos.
- Regulator workshops.
- Alto product story around disinformation as a material financial risk.
