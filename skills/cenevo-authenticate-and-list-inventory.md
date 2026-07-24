---
name: Authenticate and browse Labguru inventory
description: Create a Labguru API session token and list scientific inventory (antibodies, cell lines, plasmids, boxes, stocks) with pagination.
api: openapi/cenevo-labguru-v1-openapi.yml
operations:
- POST /api/v1/sessions
- GET /api/v1/antibodies
- GET /api/v1/cell_lines
- GET /api/v1/plasmids
- GET /api/v1/boxes
- GET /api/v1/stocks
---

# Authenticate and browse Labguru inventory

Base URL: `https://my.labguru.com/api/v1`

## 1. Get a session token
`POST /api/v1/sessions` with a JSON body `{ "login": "<email>", "password": "<password>", "account_id": <optional workspace id> }`. The response returns a token string. SSO customers generate a token from Labguru account settings instead of sending a password.

## 2. Call inventory endpoints with the token
Every subsequent request requires the token as the **`token` query parameter** (not a header). For example, list antibodies:
`GET /api/v1/antibodies?token=<TOKEN>&page=1&meta=true`

The same shape applies to other biocollections and storage:
- `GET /api/v1/cell_lines`
- `GET /api/v1/plasmids`
- `GET /api/v1/boxes`
- `GET /api/v1/stocks`

## 3. Paginate
List endpoints use page-number pagination: pass `page` (1-based) and set `meta=true` to receive pagination metadata alongside the collection.

## Error handling
- `401 Unauthorized` — token missing/invalid/expired; mint a fresh one via `/sessions`.
- `404 Not Found` — bad id or `page` out of range.
See `errors/cenevo-problem-types.yml` and `conventions/cenevo-conventions.yml`.
