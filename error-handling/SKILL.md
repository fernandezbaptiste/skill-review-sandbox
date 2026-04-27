---
name: error-handling
description: handle errors well
---

# Error Handling

Use this skill when reviewing or writing code that needs to handle unexpected failures.

## Don't swallow errors

Catching an error and silently continuing is almost always wrong. At minimum, log the error with enough context to understand what happened.

## Fail fast at boundaries

At system boundaries (API entry points, external service calls, file I/O), validate inputs and reject bad ones loudly. Inside trusted internal code, prefer assumptions over re-validation.

## Distinguish recoverable from unrecoverable

Some errors are recoverable: a network blip, a rate-limited downstream service. Retry these (with backoff). Some are not: a config file that doesn't exist, a database with a corrupt schema. Surface those immediately.

## Logging

When you log an error, include:

- The error type and message
- A request ID or correlation ID
- The relevant inputs (sanitized)

Don't just write `console.error('something went wrong')`. That's useless.

## User-facing errors

If the error reaches a user, show a helpful message. Tell them what happened and what they can do about it. Don't expose internal stack traces or details.
