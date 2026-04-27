---
name: incident-response
description: Manages production incident response workflows from initial triage through postmortem. Guides responders through alert acknowledgment, mitigation, status communication, and follow-up. Use when the user mentions outages, service degradation, on-call pages, incident response, postmortems, error rate spikes, latency issues, or on-call escalation. Covers rollback decisions, stakeholder updates, and structured post-incident review.
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

Post status updates every 15 minutes in the incident channel and on the public status page. Use this template (save to a shared runbook file such as `STATUS_UPDATE_TEMPLATE.md` for team customization):

- Current impact (e.g., "10% of API requests failing")
- What's been tried so far
- Next steps and ETA

## Step 4: Resolve and learn

When the incident is contained, do stuff to follow up. Things like writing a postmortem, scheduling a review, etc.

**Immediate (within 1 hour of resolution):**
- [ ] Post an all-clear message in the incident channel and on the status page
- [ ] Record the incident timeline (key events, decisions, and actors) while fresh
- [ ] Assign a postmortem owner

**Short-term (within 3 business days):**
- [ ] Schedule the postmortem review meeting
- [ ] Draft the postmortem using `POSTMORTEM_TEMPLATE.md` (create this file if it does not exist; see outline below)
- [ ] Identify and file action items in the issue tracker with owners and due dates

**Postmortem outline** (extract to `POSTMORTEM_TEMPLATE.md` for reuse):
- **Summary:** One-paragraph description of the incident and its impact
- **Timeline:** Chronological list of events from first alert to resolution
- **Root Cause:** What actually caused the issue
- **Contributing Factors:** What made detection or resolution harder
- **Action Items:** Specific, assigned, time-bound follow-up tasks

**Review meeting checklist:**
- Walk through the timeline without blame
- Validate that each action item is concrete and assigned
- Confirm on-call runbooks or alerts need updating based on this incident

## External communication

If the incident affected external customers, you should also let them know somehow. Maybe send an email or update the status page. Don't forget to be honest about what happened.

If a customer was particularly affected, reach out to them directly with a more detailed explanation. Be nice.

## Severity levels

We use a few severity levels:

- SEV1 — really bad
- SEV2 — bad
- SEV3 — not great but ok
