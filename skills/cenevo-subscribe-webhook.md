---
name: Subscribe to Labguru webhooks
description: List available webhook trigger keys and create/manage a webhook subscription that POSTs to your endpoint.
api: openapi/cenevo-labguru-v1-openapi.yml
operations:
- POST /api/v1/sessions
- GET /api/v1/webhooks/list
- POST /api/v1/webhooks
- GET /api/v1/webhooks
- PUT /api/v1/webhooks/{id}
- DELETE /api/v1/webhooks/{id}
---

# Subscribe to Labguru webhooks

Base URL: `https://my.labguru.com/api/v1`

## 1. Authenticate
`POST /api/v1/sessions` to get a token; pass it as `?token=<TOKEN>`.

## 2. Discover available triggers
`GET /api/v1/webhooks/list?token=<TOKEN>` returns the available `trigger_key` values (e.g. `knowledgebase_document.signed`).

## 3. Create a subscription
`POST /api/v1/webhooks?token=<TOKEN>` with an item-wrapped body:
```json
{ "token": "<TOKEN>", "item": { "trigger_key": "knowledgebase_document.signed", "active": true, "url": "https://your-endpoint.example.com/hook" } }
```
Subscriptions are **inactive by default** — set `active: true`. When the event fires, Labguru POSTs a JSON payload to `url`.

## 4. Manage subscriptions
- `GET /api/v1/webhooks` — list your subscriptions.
- `PUT /api/v1/webhooks/{id}` — update (e.g. toggle `active` or change `url`).
- `DELETE /api/v1/webhooks/{id}` — remove.

See `asyncapi/cenevo-labguru-webhooks.yml` for the full webhook surface.
