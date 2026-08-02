---
name: testerarmy-pull-request-run
description: Trigger a TesterArmy run from CI/CD for a pull request or deployment, or via a project/group webhook, and cancel or re-run batches.
api: openapi/testerarmy-openapi-original.json
operations:
  - POST /v1/projects/{projectId}/pull-request-runs
  - POST /v1/webhook/{webhookId}/{secret}
  - POST /v1/groups/webhook/{webhookId}/{secret}
  - POST /v1/batches/{batchId}/rerun
  - POST /v1/batches/{batchId}/cancel
generated: '2026-07-21'
method: generated
---

# Trigger a TesterArmy run from CI/CD

Use this to gate pull requests or deployments on QA runs.

1. **Dynamic PR run** - `POST /v1/projects/{projectId}/pull-request-runs` with the
   PR/deployment context (returns 202 Accepted). Handles `413` when an uploaded
   build is too large.
2. **Webhook triggers** - call `POST /v1/webhook/{webhookId}/{secret}` (project) or
   `POST /v1/groups/webhook/{webhookId}/{secret}` (group) from any CI system.
   See `asyncapi/testerarmy-webhooks.yml`.
3. **Re-run a batch** - `POST /v1/batches/{batchId}/rerun` to re-execute a batch.
4. **Cancel** - `POST /v1/batches/{batchId}/cancel` or `POST /v1/runs/{id}/cancel`
   to stop a queued/running batch or run (`409` if it is no longer cancellable).

Results are delivered to GitHub PR checks, Slack, Discord, or Email per your
integrations. Base URL `https://tester.army/api`; bearer API-key auth.
