---
name: error-handling
description: Guides Claude in reviewing and writing robust error-handling code. Covers distinguishing recoverable from unrecoverable errors, implementing retry-with-backoff patterns, structured error logging with correlation IDs, and crafting helpful user-facing error messages. Use when the user asks about error handling, exception management, try-catch patterns, error logging, retry logic, or debugging runtime failures in any codebase.
---

# Error Handling

Use this skill when reviewing or writing code that needs to handle unexpected failures.

## Decision tree: which pattern applies?

When you encounter an error, ask in order:

1. **Is it a transient failure?** (network blip, rate limit, timeout) → retry with exponential backoff.
2. **Is it a user input error?** (validation failure, bad request) → return a clear user-facing message; do not retry.
3. **Is it a permanent system error?** (missing config, corrupt schema, programming bug) → fail fast; do not retry.
4. **Does it cross a system boundary?** (API entry point, external call, file I/O) → validate and reject loudly before processing.
5. **Everything else** → log with full context and re-throw; never silently swallow.

---

## Don't swallow errors

Catching an error and silently continuing is almost always wrong. At minimum, log the error with enough context to understand what happened.

**Bad:**
```js
try {
  await saveRecord(record);
} catch (err) {
  // ignore
}
```

**Good:**
```js
try {
  await saveRecord(record);
} catch (err) {
  logger.error({ err, recordId: record.id, requestId: ctx.requestId }, 'Failed to save record');
  throw err; // re-throw unless you have a deliberate fallback
}
```

## Fail fast at boundaries

Validate inputs at system boundaries and reject bad ones loudly. Inside trusted internal code, prefer assumptions over re-validation.

## Distinguish recoverable from unrecoverable

**Retry-with-backoff pattern:**
```js
async function withRetry(fn, { maxAttempts = 3, baseDelayMs = 200 } = {}) {
  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await fn();
    } catch (err) {
      if (!isTransient(err) || attempt === maxAttempts) throw err;
      await sleep(baseDelayMs * 2 ** (attempt - 1));
    }
  }
}
```

## Logging

When you log an error, include:

- The error type and message
- A request ID or correlation ID
- The relevant inputs (sanitized)

**Bad:**
```js
console.error('something went wrong');
```

**Good:**
```js
logger.error(
  { err, requestId: ctx.requestId, userId: ctx.userId },
  'Payment charge failed'
);
```

## User-facing errors

If the error reaches a user, show a helpful message. Tell them what happened and what they can do about it. Never expose internal stack traces or system details.

**Example user-facing error response:**
```json
{
  "error": "payment_failed",
  "message": "We couldn't process your payment. Please check your card details and try again, or contact support if the problem persists.",
  "requestId": "req_abc123"
}
```
