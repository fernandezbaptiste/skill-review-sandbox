---
name: api-design
description: Guides RESTful API endpoint design, resource naming, status code selection, pagination structure, versioning strategy, and error response schemas. Use when the user asks about designing APIs, defining HTTP endpoints, REST conventions, API versioning, request/response formats, URL structure, OpenAPI/Swagger specs, or reviewing an existing API contract for best practices.
---

# API Design

Use this when someone is designing a new HTTP API or reviewing an existing one.

## Resources and naming

URLs describe resources, not actions. Plural nouns for collections, singular for items.

- `GET /users` — list
- `GET /users/{id}` — single user
- `POST /users` — create
- `PATCH /users/{id}` — partial update
- `DELETE /users/{id}` — remove

Multi-word path segments use kebab-case (`/access-tokens`).

## Status codes

Match the code to the outcome. Avoid 200 with an error body in the response.

- `200` successful read or update
- `201` resource created
- `204` deleted, no body
- `400` malformed request
- `401` missing or invalid auth
- `403` authenticated but forbidden
- `404` not found
- `409` conflict (e.g., duplicate key)
- `422` semantic validation error
- `500` unexpected server error

## Pagination

List endpoints must paginate. Return a structured envelope with the items and a cursor or offset for the next page.

```json
{
  "data": [...],
  "pagination": {
    "next_cursor": "eyJpZCI6MTAwfQ==",
    "has_more": true
  }
}
```

## Versioning

Version via URL prefix (`/v1/users`) or `Accept` header. Choose one approach and apply it consistently across the API.

## Error responses

Errors must be actionable. Return enough information for the client to recover or report a clear failure to the user.