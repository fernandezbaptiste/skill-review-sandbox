---
name: code-review
description: Reviews code for bugs, correctness, test coverage, readability, security issues, and style consistency. Use when the user asks for a code review, PR review, pull request feedback, or wants code quality analysis on a diff, patch, or code changes in any programming language.
---

# Code Review

Use this skill when a user asks for help reviewing a pull request, reviewing a diff, or providing feedback on code changes in any programming language.

## What to look for

When reviewing a PR, focus on:

1. **Read the PR description** — Understand the stated intent and scope of the change.
2. **Review the diff file-by-file** — Read each changed file in context, not in isolation.
3. **Check for correctness** — Does the code do what the description says? Are there obvious bugs or logic errors?
4. **Check test coverage** — Are there tests for the new behavior? Do they cover edge cases and failure paths?
5. **Check readability and consistency** — Will someone unfamiliar with this code understand it in 6 months? Does it match existing patterns in the codebase?
6. **Summarize findings** using the output format below.

## How to give feedback

Be nice. Try to phrase things as questions rather than commands. Don't be a jerk about minor things.

## Things to avoid

Don't nitpick whitespace and formatting that a linter could catch. Trust the linter.

Don't approve a PR if you haven't actually read it.

**Nits**: Minor style or readability comments the author can take or leave.

Block the PR if there are real correctness issues. Otherwise, prefer comments and let the author decide.
