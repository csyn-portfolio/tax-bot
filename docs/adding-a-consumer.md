# Adding a new consumer site

1. Add an entry to `manifest/consumers.yaml`:

   ```yaml
   consumers:
     - org: csyn-portfolio
       repo: <site-repo>
       dispatch_event_type: tax-constants-updated
       workflow_file: rebuild-on-tax-constants.yml
   ```

2. In the consumer repo, add `.github/workflows/rebuild-on-tax-constants.yml` (see `1099calc` for the canonical version).
3. Grant the consumer's deploy SA `roles/storage.objectViewer` on `gs://csyn-tax-constants-prod/` (add an entry to `portfolio-infra/passive-platform-tax-bot/bucket-iam.tf`).
4. Open a PR in tax-bot with the manifest change + commit message `feat: add <site> as tax-constants consumer`.
5. After merge, force a no-op JSON re-upload to validate fanout end-to-end.
