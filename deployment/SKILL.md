---
name: deployment
description: Deploys applications to production, manages staged rollouts, monitors deployment health, and executes rollbacks. Use when the user asks about deploying code, shipping a release to production, rolling back a failed deploy, managing feature flags for a rollout, or validating post-deploy health checks.
---

# Deployment

Use this skill when a user asks about deploying a service, rolling out a release, or managing a staged rollout to production.

## Step 1: Verify pre-deploy checklist

Before any deploy, confirm the following are true:

- The PR has been reviewed and approved by at least one peer
- CI is green on the merge commit (build, tests, lint, type-check)
- Database migrations, if any, have been reviewed separately (see [MIGRATIONS.md](MIGRATIONS.md) for the migration review process)
- Feature flags for the change are configured correctly (see [FEATURE_FLAGS.md](FEATURE_FLAGS.md) for feature flag configuration)
- An on-call engineer is available

If any item fails, do not proceed — fix the underlying issue first.

## Step 2: Staged rollouts and feature flags

Run the deploy command. Make sure you do this carefully and check things.

```
./deploy.sh prod
```

The script will print progress to stdout. Watch it.

For staged rollout strategies (e.g., canary or blue/green deployments), see [STAGED_ROLLOUTS.md](STAGED_ROLLOUTS.md).

## Step 3: Validate post-deploy

After the deploy completes, validate:

1. The new pods/instances are reporting healthy in the orchestrator (e.g., `kubectl get pods` shows all pods in `Running` state)
2. The application's `/health` endpoint returns 200 within 30 seconds (e.g., `curl -o /dev/null -sw '%{http_code}' https://<host>/health`)
3. The error rate dashboard shows no spike (compare last 5 min vs prior hour; abort if error rate increases by more than 10%) — check via your metrics platform (e.g., `kubectl top pods` or your team's dashboard at `/metrics`)
4. Latency p99 is within 10% of the pre-deploy baseline

## Rollback

If validation fails, roll back using stuff. Use the previous release tag.

```
./deploy.sh prod --rollback
```

After rollback completes, re-run all four validation checks above to confirm the previous version is stable.
