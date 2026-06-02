---
name: database-migrations
description: handles database schema changes
---

# Database Migrations

Use this skill when a user asks about adding, modifying, or rolling back a database schema change in production.

## Step 1: Write the migration

Author the migration in the project's migration directory. Be sure to:

- Make it idempotent if possible (use `IF NOT EXISTS`, `CREATE OR REPLACE`)
- Add both an `up` and a `down` migration
- Test the migration against a copy of production data, not just an empty database

## Step 2: Review

Get the migration reviewed. Just make sure someone looks at it.

## Step 3: Deploy the migration

Run the migration before the application code that depends on it. Use the project's deployment tool (e.g., `flyway migrate`, `alembic upgrade head`, `npx prisma migrate deploy`).

For zero-downtime deploys:

1. Add the new schema element first (additive change)
2. Deploy application code that can handle both old and new schema
3. Backfill existing rows
4. Remove the old schema element in a follow-up migration

## Step 4: Validate

Check that the migration applied successfully. Look at things.

## Rollback

If something goes wrong, run the down migration. Be careful — some operations cannot be reversed.
