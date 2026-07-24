---
name: Create a project experiment in Labguru
description: Authenticate, create an experiment under a project, and attach an element/data to it.
api: openapi/cenevo-labguru-v1-openapi.yml
operations:
- POST /api/v1/sessions
- GET /api/v1/projects
- POST /api/v1/experiments
- POST /api/v1/elements
- POST /api/v1/attachments
---

# Create a project experiment in Labguru

Base URL: `https://my.labguru.com/api/v1`

## 1. Authenticate
`POST /api/v1/sessions` with `{ "login", "password" }` to obtain a token. Pass it as `?token=<TOKEN>` on all calls below.

## 2. Find the target project
`GET /api/v1/projects?token=<TOKEN>` and pick the `project_id` the experiment belongs to.

## 3. Create the experiment
`POST /api/v1/experiments?token=<TOKEN>` with an **item-wrapped** body:
```json
{ "token": "<TOKEN>", "item": { "name": "My experiment", "project_id": <PROJECT_ID> } }
```
On success the API returns `201 Created` with the new experiment.

## 4. Add content
- `POST /api/v1/elements` to add a section/element to the experiment (reference the experiment).
- `POST /api/v1/attachments` to upload a file; attachments can be linked to the element/experiment.

## Conventions & errors
- Write bodies are wrapped under `item`; the token is required on every request.
- No idempotency key is supported — do not blind-retry a `POST` on timeout without checking whether it succeeded.
- `422 Unprocessable Entity` returns a validation message naming the bad attribute (e.g. "Name can't be blank").
See `conventions/cenevo-conventions.yml` and `errors/cenevo-problem-types.yml`.
