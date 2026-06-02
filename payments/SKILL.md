---
name: payments
description: Processes credit card charges, issues refunds, and checks account balances. Use when the user mentions payments, billing, charges, refunds, transactions, checkout, payment processing, or balance inquiries. Handles the full payment lifecycle including pre-charge validation, idempotency, error handling, and post-operation verification.
---

# Payments

This skill handles payment operations: charging cards, issuing refunds, and checking balances. Follow the validation workflows below before executing any operation.

---

## Quick Start

Always follow this sequence for any payment operation:
1. **Validate inputs** — confirm amount, currency, and identifiers are present and within allowed limits.
2. **Confirm with user** — surface a summary of the operation and wait for explicit approval before executing.
3. **Execute** — call the appropriate operation (see sections below).
4. **Verify response** — check the returned status and surface the result to the user.
5. **Handle errors** — if the operation fails, do not retry blindly; follow the error handling section.

---

## Operations Reference

### Charge a Card

**Workflow:**
1. Validate that `amount` is a positive integer (in cents), `currency` is a valid ISO 4217 code, and `payment_method_id` is present.
2. Confirm the charge details with the user before proceeding.
3. Generate or receive an idempotency key to prevent duplicate charges.
4. Execute the charge.
5. Verify the response status is `succeeded`.

**Example:**
```python
import stripe

response = stripe.PaymentIntent.create(
    amount=2000,
    currency="usd",
    payment_method="pm_card_visa",
    confirm=True,
    idempotency_key="unique-key-per-request"
)

if response["status"] != "succeeded":
    raise ValueError(f"Charge failed with status: {response['status']}")
```

**Safety constraints:**
- Always use an idempotency key.
- Never log or expose raw card numbers or CVVs.

---

### Issue a Refund

**Workflow:**
1. Validate that the `charge_id` or `payment_intent_id` exists and that the refund `amount` does not exceed the original charge.
2. Confirm the refund amount and target charge with the user before proceeding.
3. Execute the refund.
4. Verify the response status is `succeeded`.

**Example:**
```python
import stripe

response = stripe.Refund.create(
    payment_intent="pi_abc123",
    amount=1000,
    idempotency_key="refund-unique-key"
)

if response["status"] != "succeeded":
    raise ValueError(f"Refund failed with status: {response['status']}")
```

**Safety constraints:**
- Do not issue duplicate refunds — check existing refunds on the charge first.
- Refunds may take several business days to appear; inform the user.

---

### Check Balance

**Workflow:**
1. Identify whether the user wants the available balance, pending balance, or both.
2. Execute the balance lookup (read-only, no confirmation required).
3. Surface the result clearly to the user.

**Example:**
```python
import stripe

balance = stripe.Balance.retrieve()

for fund in balance["available"]:
    print(f"Available ({fund['currency'].upper()}): {fund['amount'] / 100:.2f}")

for fund in balance["pending"]:
    print(f"Pending ({fund['currency'].upper()}): {fund['amount'] / 100:.2f}")
```

---

## Reference

### Error Handling

| Error Type | Action |
|---|---|
| `card_declined` | Inform the user; do not retry without a new payment method. |
| `insufficient_funds` | Inform the user; do not retry automatically. |
| `idempotency_error` | The same key was used with different parameters; generate a new key. |
| `rate_limit_error` | Wait and retry with exponential backoff (max 3 attempts). |
| `api_connection_error` | Check network connectivity; do not assume the charge succeeded. |
| Unknown / unexpected | Surface the raw error message to the user and stop. Do not retry. |

### General Safety Rules

- **Always confirm destructive operations** (charges, refunds) with the user before executing.
- **Never exceed the user-specified amount** for any charge or refund.
- **Do not retry failed charges** without explicit user instruction.
- **Log operation IDs** (e.g., `pi_`, `re_` prefixes) for auditability.
- When in doubt, do less and ask the user for clarification.
