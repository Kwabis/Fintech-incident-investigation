# Initial Findings

## What happened?
NuePay customers received a phishing SMS impersonating NuePay ("Your account is locked.
Verify now: [https://nuewpay.com]"), leading three customers to submit their login credentials on a
fake page. The attacker used the stolen credentials to log into each account with a
password only (no OTP), and within approximately two minutes of each login, transferred
funds from each compromised account to a single external recipient account.

## When did it happen?
2026-08-20, 08:52:14 UTC (first suspicious login) through 13:44:30 UTC (last fraudulent
transaction) a roughly 5-hour window.

## Which accounts were compromised?
CUST-0231, CUST-0587, CUST-0942.

## Which logins are suspicious, and why?
Three logins on 2026-08-20 (08:52:14, 11:07:39, 13:41:56):
- `auth_method` = `password` only. Every other login on record for these same three
  customers uses `password+otp`.
- `is_new_device` = `true` and `is_new_location` = `true` on all three; every prior
  baseline login for these customers shows `false` on both flags.
- `device_id` = `DEV-ATK0001` on all three, which does not match any of the three
  customers' established device IDs (`DEV-1A2B3C`, `DEV-4D5E6F`, `DEV-7G8H9I`).

## Which transactions are unauthorized?
TXN-100999, TXN-101000, TXN-101001. All three share `recipient_id = NUE-MULE0099`, and all
three carry `source_ip = 45.155.205.12` and `device_id = DEV-ATK0001`, an exact match to the
attacker login sessions identified above. This match is what links the suspicious logins
directly to the fraudulent transfers.

## What IP/device/location is suspicious?
`source_ip 45.155.205.12`, `device_id DEV-ATK0001`, and location "Amsterdam, Netherlands"
appear across all three compromised accounts' suspicious logins and transactions. The three
victims' legitimate logins originate from three different Ghanaian locations (Accra,
Kumasi, Accra) on three distinct device IDs — a single IP/device pair touching three
unrelated customer accounts inside a 5-hour window is the core anomaly.

## Preliminary IOCs
- IP address: `45.155.205.12`
- Device ID: `DEV-ATK0001`
- Location: Amsterdam, Netherlands
- Recipient/mule account: `NUE-MULE0099`
- Auth pattern: password-only login (OTP absent) on accounts that otherwise always authenticate with password+otp