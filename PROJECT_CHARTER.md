# Capstone Project Charter

## Project Title
Investigation of Unauthorized Transactions Caused by Compromised Fintech Customer Accounts

## Track
Incident Investigation

## Attack Vector
Phishing (SMS-based credential theft)

## Fictional Organization
**NuePay** — mobile P2P payment fintech, ~1,200 active customers.

## Incident Summary
Attacker sends an SMS impersonating NuePay: *"Your account is locked. Verify now: [fake link]"*.
Within a single 6-hour campaign window on 2026-08-20, three customers (CUST-0231, CUST-0587,
CUST-0942) enter credentials on a fake login page. The attacker, using one IP/device
(45.155.205.12 / DEV-ATK0001), logs into each account minutes later. Because OTP enforcement
was not consistently applied to all login paths, single-factor (password-only) access was
sufficient. The attacker transfers funds from each account to a shared external account
(NUE-MULE0099) within roughly two minutes of each login, before any customer notices.

## Root Cause (working hypothesis, to be confirmed by analysis)
OTP/2FA existed as a control but was not enforced on every authentication path, allowing a
phished password alone to grant full account access.

## Objectives (from approved proposal)
- Reconstruct the phishing vector: how the message was delivered, what it impersonated, how
  it induced credential disclosure
- Trace the attack path from credential theft to account access and unauthorized
  transactions across multiple accounts
- Analyze authentication and transaction evidence to determine detection and impact
- Assess the security weaknesses that enabled the compromise (e.g. lack of MFA/OTP
  enforcement, weak login anomaly detection)
- Confirm the activity is externally driven rather than insider-caused
- Recommend practical containment, remediation, and security-awareness controls

## Scope
In scope: the 3 compromised accounts, the single campaign window, the phishing-to-transfer
chain, relevant authentication and transaction logs, and the security controls directly
related to the incident.
Out of scope: broader NuePay infrastructure, unrelated customers, redesigning NuePay's
full security architecture, anything not evidenced in the two log files.

## Final Report Structure (fixed, per approved template)
1. Title Page
2. Executive Summary (3-5 sentences, plain language)
3. Background and Scope
4. Incident Timeline — chronological, timestamped, includes at least one ATT&CK
   tactic/technique reference inline (IOC and MITRE working docs feed this section;
   they are not separate report sections)
5. Root Cause
6. Impact (CIA triad)
7. Response Actions Taken
8. AI Integration (required — must be genuine analysis use, not just writing/formatting help)
9. Recommendations (specific, named fixes)
10. Evidence (3+ screenshots + 1 diagram, each captioned)
11. Reflection (2-3 sentences: which skills this relied on, what you'd change)

## Evidence Base
- `authentication_logs.csv` — 25 events (baseline + noise + 3 attacker logins)
- `transaction_logs.csv` — 14 events (baseline + 3 fraudulent transfers)

## Deliverables
1. `initial_findings.md`
2. `incident_timeline.md`
3. `iocs.md`
4. `mitre_mapping.md`
5. `root_cause.md`
6. `impact_analysis.md`
7. AI analysis (input, output, verification, screenshot)
8. 3+ evidence screenshots + 1 incident-flow diagram, captioned
9. Final report (all sections)
10. Presentation deck (8 slides)

## Status
- [x] Scenario locked
- [x] `authentication_logs.csv` built
- [x] `transaction_logs.csv` built
- [ ] `initial_findings.md`
- [ ] Everything below (see sequence)

## Sequence — What Comes Next, In Order

| # | Step | Depends on | Produces |
|---|---|---|---|
| 1 | Scenario | — | ✅ this charter |
| 2 | Auth logs | Scenario | ✅ `authentication_logs.csv` |
| 3 | Transaction logs | Auth logs | ✅ `transaction_logs.csv` |
| 4 | **Initial findings** (what/when/who/what evidence) | Both CSVs | `initial_findings.md` |
| 5 | Timeline (chronological correlation) | Initial findings | `incident_timeline.md` |
| 6 | IOC list (4-5, well-supported) | Timeline | `iocs.md` |
| 7 | MITRE ATT&CK mapping | IOCs | `mitre_mapping.md` |
| 8 | Evidence screenshots + diagram | Timeline + IOCs | `evidence/*.png` |
| 9 | Root cause | Timeline + IOCs | `root_cause.md` |
| 10 | Impact (CIA triad) | Root cause | `impact_analysis.md` |
| 11 | Response + recommendations | Root cause + Impact | section for report |
| 12 | AI component (real run, real verification) | Timeline/IOCs available to feed the AI tool | `ai_analysis.md` + screenshot |
| 13 | Report writing | Everything above exists | `final_report` (draft) |
| 14 | Slide deck | Final report | `presentation.pptx` |
| 15 | Cross-check report ↔ slides | Both exist | consistency pass |
| 16 | Final check + GitHub structure + defense rehearsal | Everything | submission-ready |

