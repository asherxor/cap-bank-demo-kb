# Incident Playbook – Mobile Banking Outage

## Scope

- Service: Mobile banking app (iOS / Android)
- Impact: Customers cannot log in or complete transactions
- Criticality: High

## Objectives

1. Restore service as quickly as possible.
2. Preserve integrity of customer data and funds.
3. Maintain narrative control and prevent disinformation.

## Triage Steps

1. Check CAP evidence/attestations for the latest deployment of the mobile backend.
2. Verify SBOM and attestations for the current release:
   - Confirm no unauthorized components were deployed.
   - Confirm all critical dependencies are patched.
3. Correlate application logs, infrastructure metrics, and threat intelligence feeds.

## Decision Points

- If outage is caused by known change → trigger change rollback procedure.
- If outage is unexplained and correlated with malicious indicators →
  activate **Major Incident** and involve security operations.

## Communication

- Use `communication_policy_crisis.md` for external messaging.
- Internal updates to:
  - Executive team (every 30 minutes).
  - Customer support (playbook for frontline messaging).
