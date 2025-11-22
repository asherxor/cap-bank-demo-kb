# Incident Response Policy – Bank Demo

## Purpose

Define how the bank detects, responds to, and recovers from security and operational incidents.

## Scope

- All production systems that handle customer data or financial transactions.
- All staff and third-party service providers.

## Roles & Responsibilities

- **CISO** – Owns the incident response program.
- **Incident Manager** – Coordinates response for major incidents.
- **Communications Lead** – Owns external and internal messaging during incidents.

## Incident Severity Levels

- SEV-1 – Critical impact (service unavailable, high financial or reputational risk).
- SEV-2 – Major degradation or localized impact.
- SEV-3 – Minor or potential incident.

## Required Actions for SEV-1

1. Declare incident and assign an Incident Manager.
2. Open an Incident Record in CAP and attach:
   - SBOM and attestation for the affected service.
   - Any relevant KnowledgeArtifacts (playbooks, runbooks).
3. Trigger tabletop exercise or real-time coordination using CAP evidence/attestations as the source of truth.
4. Follow communication guidelines from `communication_policy_crisis.md`.
