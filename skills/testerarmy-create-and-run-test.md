---
name: testerarmy-create-and-run-test
description: Create a TesterArmy project, define a natural-language test, and queue a remote run in the TesterArmy cloud, then read the result.
api: openapi/testerarmy-openapi-original.json
operations:
  - POST /v1/projects
  - POST /v1/tests
  - POST /v1/tests/{testId}/runs
  - GET /v1/runs/{id}
generated: '2026-07-21'
method: generated
---

# Create and run a TesterArmy test

Authenticate every request with `Authorization: Bearer <API_KEY>` (see
`authentication/testerarmy-authentication.yml`). Base URL: `https://tester.army/api`.

1. **Create a project** - `POST /v1/projects` with `{"name","url","projectType":"web"}`.
   Capture the returned `projectId`.
2. **(optional) Register an environment** - `POST /v1/projects/{projectId}/environments`
   with a name (e.g. Staging) and URL, so runs can target `--env`.
3. **Create a test** - `POST /v1/tests` with `projectId`, a `title`, and `steps[]`
   using `act` / `assert` / `login` step types (natural language, not selectors).
4. **Trigger a run** - `POST /v1/tests/{testId}/runs` (returns 202 Accepted; the run
   queues in the TesterArmy cloud).
5. **Poll the result** - `GET /v1/runs/{id}` until the run reaches a terminal status;
   read screenshots/recordings/verdict from the response.

Every operation returns `429` when rate limited (honor Retry-After) and `401` on a
missing/invalid key. See `errors/testerarmy-problem-types.yml` and
`conventions/testerarmy-conventions.yml`.
