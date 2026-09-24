# Response Actions Taken

Actions in order of execution once the compromise was detected.

1. **Contain the accounts.** Immediately block CUST-0231, CUST-0587, and CUST-0942 from
   further withdrawals or transfers, preventing any additional funds from leaving these
   accounts while the investigation proceeds.

2. **Attempt to trace and flag the destination.** Trace the transferred funds to the
   recipient account, `NUE-MULE0099`, and flag or freeze that account within NuePay's
   platform if technically possible, since all three fraudulent transfers went to this
   single recipient.

3. **Verify the message source internally.** Confirm with NuePay's own messaging and
   marketing systems that the "Your account is locked" SMS did not originate from
   NuePay, ruling out an internal error and confirming the phishing message was sent by
   an external attacker.

4. **Reset credentials.** Force a password reset and require re-enrollment of OTP for
   all three affected accounts before access is restored, ensuring the attacker's
   knowledge of the old password is no longer usable.

5. **Review account activity.** Review each of the three accounts for any further
   unauthorized activity beyond the confirmed transfer, and confirm no additional
   accounts show the same IP/device pattern (`45.155.205.12` / `DEV-ATK0001`).

6. **Restore safe access.** Once credentials are reset and OTP re-enrolled, unblock the
   three accounts so the legitimate customers can resume normal use.