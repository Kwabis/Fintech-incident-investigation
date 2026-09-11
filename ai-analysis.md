# AI-Assisted Analysis

## Tool Used
ChatGPT

## Input
`authentication_logs.csv` only (transaction data was not provided to the AI at this
stage). Prompt: identify suspicious login activity in the log.

## AI Output (summary)
The AI flagged the three logins from `45.155.205.12` / `DEV-ATK0001` (Amsterdam),
against CUST-0231 (08:52), CUST-0587 (11:07), and CUST-0942 (13:41), as the strongest
indicator, noting that all three used `auth_method=password` only while each customer's
normal logins use `password+otp`.

It separately reviewed two failed-then-successful login pairs (CUST-1103, CUST-0450) and
did not flag them as malicious, noting they occurred on the same device, IP, and
location as each customer's normal activity, consistent with an ordinary mistyped
password followed by a correct retry.

## Human Verification

**Confirmed correct:**
- The IP/device/location pattern across all three accounts matches the IOCs already
  identified independently in `initial_findings.md`.
- The `password` vs `password+otp` distinction as the key anomaly signal matches the
  root cause already documented in `root_cause.md`.
- Correctly did not flag CUST-1103 and CUST-0450 as malicious. Both are legitimate
  authentication retries deliberately included in the dataset as noise, distinguishing
  them from the real incident required checking device/IP/location consistency, which
  the AI did correctly.

**Missed or out of scope (required human follow-up):**
- The AI was given only the authentication log, so it could not connect the three
  suspicious logins to the corresponding unauthorized transactions in
  `transaction_logs.csv`, the shared recipient account `NUE-MULE0099`, or the
  approximately two-minute delay between each login and transfer. This correlation was
  performed manually across both files.
- No ATT&CK mapping (T1566, T1078) was produced by the AI; this was added through human
  analysis.
- No determination of externally-driven versus insider activity was produced by the AI;
  this was reasoned separately from the login geography and lack of prior IP history.

## Conclusion
The AI tool correctly surfaced the primary anomaly pattern from raw authentication data
and correctly avoided false positives on benign retry events. It did not perform
cross-file correlation, threat-framework mapping, or attribution reasoning, all of which
required human analysis on top of its output. This session is evidenced by the attached
screenshot (see `evidence/ai_evidence.png`).