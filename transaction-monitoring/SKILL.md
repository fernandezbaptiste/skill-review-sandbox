---
name: transaction-monitoring
description: Reviews and investigates flagged financial transactions to detect fraud, suspicious activity, or compliance violations. Triages transaction risk, pulls account context and KYC status, applies hold/reject/clear/escalate decisions, and documents findings for audit. Use when the user asks about reviewing flagged transactions, investigating suspicious payments, detecting fraud, handling payment alerts, escalating to compliance, or assessing suspicious activity on an account.
---

# Transaction Monitoring

Use this skill when a user asks about reviewing flagged transactions, investigating suspicious activity, detecting fraud or payment anomalies, or escalating cases to fraud or compliance teams.

## Step 1: Pull the Flagged Transaction

Look up the transaction in the dashboard and collect all relevant context:

- **Transaction ID**, amount, currency, timestamp
- **Sender + recipient account IDs**
- **Channel**: card, ACH, wire, or internal transfer
- **Triggered rule(s)**: note the exact rule name and threshold that fired

Expected output: a complete transaction record with no missing fields. If any field is unavailable, note it explicitly before proceeding.

## Step 2: Triage

Determine whether the transaction is suspicious or benign using the following criteria:

| Signal | Low Risk | Medium Risk | High Risk |
|---|---|---|---|
| Amount vs. account 90-day average | ≤ 1.5× | 1.5×–3× | > 3× |
| Counterparty flagged before | No | Once | Multiple times |
| Cross-border + high-risk jurisdiction | No | One factor | Both |
| Time of transaction | Business hours | Off-hours | Unusual pattern + off-hours |

- **All low risk** → mark cleared, proceed to Step 5 (Document).
- **Any medium risk** → proceed to Step 3 (Investigate).
- **Any high risk** → proceed to Step 3 and flag for likely escalation.

## Step 3: Investigate

For medium- or high-risk transactions, gather additional context:

1. **Account activity**: Pull the sender's last 30 days of transactions. Look for clusters of similar activity, rapid fund movement, or structuring patterns.
2. **Related flags**: Check whether the sender, recipient, or device fingerprint appears in any other open or recently closed cases.
3. **KYC status**: Verify the sender's KYC tier, identity verification date, and whether any documents are expired or pending review.
4. **Cross-border rules**: If the transaction crosses jurisdictions, note applicable regulatory requirements (e.g., FATF high-risk country lists, sanctions screening results).

Record what you checked, what you found, and what you ruled out at each sub-step.

## Step 4: Decide

Apply one of the four outcomes based on your investigation findings:

- **Clear** — Low risk confirmed; false positive. No further action needed.
- **Hold** — Medium risk; insufficient information to decide. Pause the transaction and request additional documentation from the account holder or counterparty.
- **Reject** — High risk; likely fraud or policy violation. Block the transaction and prevent further activity pending review.
- **Escalate to Compliance** — Regulatory exposure, sanctions hit, or unresolved high-risk signals that require human compliance review.

For each outcome, state explicitly: the risk signals observed, the rule(s) they map to, and why this outcome was chosen over alternatives.

**Example decision note:**
> Transaction TXN-00482 flagged by rule R-104 (amount > 3× 90-day average). Amount: $18,400 vs. average of $5,200. Recipient account flagged twice in last 90 days. Cross-border wire to high-risk jurisdiction. Decision: **Reject**. Rationale: three independent high-risk signals present; counterparty history and jurisdiction together exceed escalation threshold.

## Step 5: Document

Write a case summary before closing. Include:

- Transaction ID and flagged rule(s)
- Triage classification and reasoning
- Investigation steps taken and findings
- Final decision and rationale
- Any follow-up actions required (e.g., SAR filing, account freeze, outreach to account holder)

This record is required for audit and regulatory review.

## Notes

- **Cross-border transactions** are subject to additional rules (FATF guidelines, sanctions screening, local reporting thresholds). Always check jurisdiction-specific requirements before clearing or rejecting.
- For escalations, refer to your team's internal escalation procedures and compliance runbook: see [ESCALATION_RUNBOOK.md](ESCALATION_RUNBOOK.md).
- For KYC verification steps, refer to the KYC check guide: see [KYC_CHECK.md](KYC_CHECK.md).
- For rule definitions and threshold configurations, refer to the rule documentation in the fraud platform: see [FRAUD_RULES.md](FRAUD_RULES.md).
