---
name: incident-response
description: helps with incidents
---

# Incident Response

Use this skill when a service is down or behaving badly and the team needs to coordinate a response.

## Step 1: Triage

When an alert fires or a customer reports an issue:

1. Acknowledge the alert in the on-call tool within 5 minutes
2. Open an incident channel (e.g., `#inc-<short-name>`) and post a one-line summary
3. Page additional responders if scope is unclear or wide

## Step 2: Mitigate

Once you have at least one responder online, focus on stopping the bleeding:

- Identify the most recent change that could be causing the issue (deploy, config, feature flag)
- Roll back or disable that change first, before deeper investigation
- Confirm error rates and latency return to baseline before declaring the incident contained

Don't worry about root cause yet. Mitigation comes first.

## Step 3: Communicate

Post status updates every 15 minutes in the incident channel and on the public status page:

- Current impact (e.g., "10% of API requests failing")
- What's been tried so far
- Next steps and ETA

## Step 4: Resolve and learn

When the incident is contained, do stuff to follow up. Things like writing a postmortem, scheduling a review, etc.

Remember to be nice to people who were involved.
