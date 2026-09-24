# Incident Timeline

## Summary
All timestamps UTC, 2026-08-20. Sequence covers the three phishing-driven account
compromises and their corresponding unauthorized transfers.

## Chronological Events

| Time | Event | Detail | Source |
|---|---|---|---|
| 08:51:02 | Failed login attempt, CUST-0231 | Attacker mistypes stolen password on first attempt | `authentication_logs.csv` |
| 08:52:14 | **Suspicious login, CUST-0231** | Login succeeds from `45.155.205.12` / `DEV-ATK0001` (Amsterdam), `auth_method=password` only, `is_new_device=true`, `is_new_location=true` | `authentication_logs.csv` |
| 08:54:02 | **Unauthorized transaction, CUST-0231** | GHS 4,800.00 transferred to `NUE-MULE0099` from same IP/device, 1m48s after login | `transaction_logs.csv` |
| 11:07:39 | **Suspicious login, CUST-0587** | Login succeeds from `45.155.205.12` / `DEV-ATK0001`, `auth_method=password` only, `is_new_device=true`, `is_new_location=true` | `authentication_logs.csv` |
| 11:09:15 | **Unauthorized transaction, CUST-0587** | GHS 3,950.00 transferred to `NUE-MULE0099` from same IP/device, 1m36s after login | `transaction_logs.csv` |
| 13:41:56 | **Suspicious login, CUST-0942** | Login succeeds from `45.155.205.12` / `DEV-ATK0001`, `auth_method=password` only, `is_new_device=true`, `is_new_location=true` | `authentication_logs.csv` |
| 13:44:30 | **Unauthorized transaction, CUST-0942** | GHS 5,200.00 transferred to `NUE-MULE0099` from same IP/device, 2m34s after login | `transaction_logs.csv` |

## Attack Path (ATT&CK-mapped)

1. **Credential theft via phishing.** All three customers had previously authenticated
   only from their own established devices and IP addresses (Accra and Kumasi, Ghana)
   using `password+otp`. The precursor SMS phishing message that led to credential
   disclosure corresponds to **MITRE ATT&CK T1566 (Phishing)**.
2. **Account access with stolen credentials.** Each of the three logins above used
   `auth_method=password` only, no OTP, on an unrecognized device and location, the
   opposite of every prior login for these customers. Gaining account access this way,
   using legitimate but stolen credentials rather than exploiting a technical
   vulnerability, corresponds to **MITRE ATT&CK T1078 (Valid Accounts)**.
3. **Unauthorized transfer.** Within roughly two minutes of each login, funds were moved
   out to a single external recipient, `NUE-MULE0099`, common to all three incidents.

## Externally-Driven Determination

The activity is assessed as externally driven, not insider-caused, based on:
- `45.155.205.12` and `DEV-ATK0001` do not appear in any baseline/legitimate activity
  for any of the 1,200 NuePay customers in the evidence set; they appear only during
  the three incident events.
- The IP resolves to Amsterdam, Netherlands, while all three victims' established
  accounts are based in Ghana, with no travel or location-change context.
- A single IP/device pair accessed three otherwise-unconnected customer accounts within
  a 5-hour window, a pattern consistent with externally-controlled attack infrastructure
  rather than any individual customer's or employee's normal behavior.

## Not Covered Here
Root cause, impact, and recommendations are addressed in their own documents
(`root_cause.md`, `impact_analysis.md`) and are not restated in this timeline.