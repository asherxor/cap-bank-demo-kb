# Use Case UC01 – Mobile Banking Outage (Bank / Alto Demo)

## 1. Scenario Summary

During a busy weekday morning:

- The mobile banking app becomes **unavailable** for a large portion of customers.
- Social media reports:
  - “The app is down again.”
  - “I can’t see my salary.”
- Internal monitoring confirms a significant outage affecting the mobile backend API.

The bank activates its **incident response process** and uses **CAP** as the evidentiary backbone.

---

## 2. Actors

- **Bank Operations / SRE**
- **Security Operations (SOC)**
- **CISO / Risk**
- **Communications / PR**
- **Alto / External intel partner**
- **CAP Platform (backend + Canon)**

---

## 3. Pre-Conditions

- Mobile banking backend is integrated with CAP:
  - SBOMs generated on each deployment.
  - Attestations created and anchored for each release.
- Policies and playbooks:
  - `policies/incident_response_policy.md`
  - `policies/communication_policy_crisis.md`
  - `kb/incident_playbook_mobile_outage.md`

- Org Agents configured in CAP for:
  - “Service outage triage”.
  - “Customer impact analysis”.

---

## 4. Trigger

- Monitoring alerts SEV-1 incident: “Mobile banking API error rate > 80%”.
- Customer complaints spike.
- Incident Manager declares **SEV-1 Mobile Banking Outage**.

---

## 5. Flow with CAP

### Step 1 – Triage and Evidence Gathering

1. Operations team opens CAP and:
   - Queries recent **SBOMs** and **attestations** for `service:mobile-banking`.
2. They verify:
   - Last deployment timestamp.
   - Attestation status (`verified`).
   - Canon anchor status (`valid=true`).

### Step 2 – Run Org Agent for Outage Analysis

1. An Org Agent is invoked with context:
   - SBOM + attestation data from CAP.
   - `kb/incident_playbook_mobile_outage.md`.
   - Relevant policies.
2. The Agent:
   - Summarizes likely root causes (deployment vs infra vs dependency).
   - Proposes immediate checks and rollback decisions.

### Step 3 – Promote Agent Run to Evidence

- The Agent run is promoted via CAP to:
  - Evidence (`kind=ai_assessment`).
  - Attestation (`type=agent_run`) tied to the incident.
- This provides a **recorded decision trail** for later review.

### Step 4 – Communication

- Communications team:
  - Uses `communication_policy_crisis.md` to draft updates.
  - Ensures all claims are backed by CAP evidence (e.g., no data loss).

### Step 5 – Recovery & Post-Incident Review

- After service restoration:
  - Another Org Agent run generates a **post-incident report**.
  - The report is again promoted to Evidence/Attestation.
  - Lessons learned are added/updated in `kb/` and `policies/`.

---

## 6. Outcomes

- The bank can **prove**:
  - What was deployed during the incident window.
  - Which checks were performed.
  - That no unauthorized changes occurred (if true).
- Alto and other partners can:
  - Consume CAP evidence to generate higher-level insights.
  - Integrate the incident into broader threat decision architectures.

This use case is suitable for live demos and tabletop exercises.
