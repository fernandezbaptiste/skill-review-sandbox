# skill-review-sandbox

Sandbox for testing [tesslio/skill-review-and-optimize#23](https://github.com/tesslio/skill-review-and-optimize/pull/23) — per-skill `/apply-optimize`.

## Setup (one-time)

1. Add a repo secret named `TESSL_API_TOKEN`:
   - Go to [Settings → Secrets and variables → Actions](https://github.com/fernandezbaptiste/skill-review-sandbox/settings/secrets/actions/new)
   - Get a token from [tessl.io/account/api-keys](https://tessl.io/account/api-keys)

## How to test

1. The repo has two skills: `payments/SKILL.md` and `onboarding/SKILL.md` — both intentionally rough.
2. A PR is already open that touches both. The action runs against PR commits.
3. The bot comment will show two optimized blocks, each with a per-skill CTA.

### Things to try

- `/apply-optimize payments/SKILL.md` — should apply only payments
- `/apply-optimize` (bare) — should apply all
- `/apply-optimize bogus/SKILL.md` — should reply with available paths

## Cleanup

Delete this repo when done: `gh repo delete fernandezbaptiste/skill-review-sandbox --yes`
