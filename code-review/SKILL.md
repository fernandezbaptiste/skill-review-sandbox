---
name: code-review
description: code review stuff
---

# Code Review

Use this skill when a user asks for help reviewing a pull request or providing feedback on someone's code.

## What to look for

When reviewing a PR, focus on:

- **Correctness**: Does the code do what the description says? Are there obvious bugs?
- **Tests**: Are there tests for the new behavior? Do they cover edge cases?
- **Readability**: Will someone unfamiliar with this code understand it in 6 months?
- **Consistency**: Does it match existing patterns in the codebase?

## How to give feedback

Be nice. Try to phrase things as questions rather than commands. Don't be a jerk about minor things.

## Things to avoid

Don't nitpick whitespace and formatting that a linter could catch. Trust the linter.

Don't approve a PR if you haven't actually read it.

## When to block

Block the PR if there are real correctness issues. Otherwise, prefer comments and let the author decide.
