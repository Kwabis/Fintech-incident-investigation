# Root Cause

## Causal Chain

1. SMS phishing message impersonating NuePay ("Your account is locked. Verify now")
   delivered to customers.
2. Three customers (CUST-0231, CUST-0587, CUST-0942) entered their login credentials on
   the fake page, resulting in credential disclosure to the attacker.
3. Attacker used the disclosed password to authenticate to each account.
4. **Authentication succeeded using password only, with no OTP presented** — see
   `auth_method=password` on all three incident rows in `authentication_logs.csv`,
   versus `auth_method=password+otp` on every other login on record for these same
   three customers.
5. Because password-only authentication was accepted, the stolen credential alone was
   sufficient to gain full account access, no possession of the customer's phone or
   OTP device was required.
6. Account access enabled the attacker to initiate transfers, executed within roughly
   2 minutes of login (see `incident_timeline.md`), to a single external recipient
   account (`NUE-MULE0099`).

## Specific Root Cause

The incident was possible because **OTP/2FA was not enforced on every login path**. The
evidence shows OTP is NuePay's normal second factor (used in 100% of legitimate logins in
the dataset), but the three attacker logins were accepted with password alone. Phishing
supplied the first factor; the absence of enforced second-factor verification is what
converted a stolen password into full account takeover.

This is a root cause statement about a specific, evidenced control gap, not a general
claim about "weak security." No other authentication path, device-binding control, or
system weakness is asserted here, because no other gap is evidenced in the available logs.

## Contributing Factor: Anomaly Detection Gap

Each attacker login was flagged by the system itself as `is_new_device=true` and
`is_new_location=true`, yet the login was still permitted to proceed to a full session
capable of initiating transfers. The system captured the anomaly signal but did not act
on it (e.g. by blocking the session, requiring step-up verification, or alerting the
customer). This is a secondary, evidenced factor: detection data existed but was not
used to prevent the compromise. 