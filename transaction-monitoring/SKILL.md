---
name: transaction-monitoring
description: monitors transactions
---

# Transaction Monitoring

Use this skill when a user asks about reviewing flagged transactions, investigating suspicious activity, or escalating to fraud / compliance.

## Step 1: Pull the flagged transaction

Look up the transaction in the dashboard. Get the relevant context:

- Transaction ID, amount, currency, timestamp
- Sender + recipient account IDs
- Channel (card, ACH, wire, internal transfer)
- The rule(s) that flagged it

## Step 2: Triage

Decide whether the transaction looks suspicious or benign. Use your judgment.

Things to consider:
- Is the amount unusual for this account?
- Has this counterparty been flagged before?
- Does the timing look weird?

If it looks fine, mark it cleared and move on.

## Step 3: Investigate

If it doesn't look fine, investigate more deeply. Pull the account's recent activity, check for related flags, look at the sender's KYC status.

Take notes as you go. Be thorough.

## Step 4: Decide

You have a few options:

- Clear the transaction (low risk, false positive)
- Hold the transaction (medium risk, needs more info)
- Reject the transaction (high risk, likely fraud)
- Escalate to compliance

Pick whichever fits.

## Step 5: Document

Write down what you did. Include why. This matters for audit later.

## Notes

Try to be careful with cross-border transactions. They have more rules.
