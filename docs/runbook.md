# tax-bot operator runbook

## Weekly drift PR review

1. Email arrives at pete@cloudsyn.net (subject: "Tax constants drift detected — YYYY-MM-DD").
2. Open the linked PR in csyn-portfolio/tax-bot.
3. Read the PR body — sources cited, diff structured, review checklist below it.
4. Verify each changed value against the primary source (IRS, SSA, Treasury).
5. If correct: approve + merge. If incorrect or premature: close the PR; the verifier will re-run next Monday.

## What happens on merge

- Cloud Build trigger syncs `data/*.json` to `gs://csyn-tax-constants-prod/`.
- GCS object-change event hits the fanout Cloud Function.
- Fanout reads `manifest/consumers.yaml`, sends `repository_dispatch` to each listed consumer.
- Each consumer's GH Actions workflow rebuilds with the new values baked in.

## Killswitch

To pause verifier runs without tearing down infra:

```
gcloud scheduler jobs pause verifier-weekly --location=us-central1 --project=csyn-tax-bot
```
