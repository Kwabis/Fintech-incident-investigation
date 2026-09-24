# Recommendations

## 1. Enforce OTP on every login path (M1032, Multi-factor Authentication)
Require OTP verification on all account logins, with no path that accepts a password
alone. This directly closes the gap identified in `root_cause.md`: the attacker's three
successful logins all used `auth_method=password` only, while every legitimate login on
record used `password+otp`. Enforcement, not availability, was the failure.

## 2. Act automatically on device/location anomaly flags (M1036, Account Use Policies)
NuePay's system already generates `is_new_device` and `is_new_location` flags, and all
three attacker logins triggered both. Currently these flags are logged but not acted on.
Recommend configuring the system to automatically require step-up verification or block
the session outright when both flags fire together, rather than allowing the login to
proceed and only recording the anomaly after the fact.

## 3. Customer phishing awareness messaging (M1017, User Training)
Send customers a standing notice, in-app and via verified channels, that NuePay will
never request account verification through an SMS link, and provide a clear way to
report suspicious messages. This does not prevent this specific incident's root cause
but reduces the likelihood of future customers falling for the same or similar phishing
campaigns.
