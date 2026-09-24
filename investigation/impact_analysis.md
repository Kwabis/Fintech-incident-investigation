# Impact Analysis (CIA Triad)

## Confidentiality: Impacted

Customer login credentials for three accounts (CUST-0231, CUST-0587, CUST-0942) were
disclosed to the attacker via the phishing page. Account-level information, balances,
transaction history, and account identifiers, was exposed once the attacker gained
access, since a logged-in session has visibility into that data.

## Integrity: Impacted

Transaction records for all three accounts were altered by the attacker initiated
transfers (TXN-100999, TXN-101000, TXN-101001). Account balances no longer reflect only
legitimate customer activity; each account's transaction history now contains an
unauthorized entry.

## Availability: Not Impacted

No evidence in `authentication_logs.csv` or `transaction_logs.csv` indicates a service
outage, denial of access, or disruption to any customer's ability to use NuePay. The
three victims retained account access throughout, and other customers' logins and
transactions on the same date (e.g. CUST-0450) proceeded normally. Availability is
excluded from this incident's impact based on the available evidence.

## Financial Impact

Three unauthorized transfers totaling GHS 13,950.00 (GHS 4,800.00 + GHS 3,950.00 +
GHS 5,200.00) were sent to a single external recipient account. This figure is scoped to
the three confirmed accounts in evidence; no claim is made about impact beyond these
three accounts.

## Scope of Impact

Limited to three customer accounts out of NuePay's approximately 1,200 active customers.
No evidence supports a broader compromise; the same anomaly pattern (single external
IP/device, password only, new device and location flags) does not appear against any
other customer_id in the dataset.