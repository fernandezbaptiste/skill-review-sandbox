---
name: api-design
description: Guides RESTful API endpoint design, resource naming, status code selection, pagination structure, versioning strategy, and error response schemas. Use when the user asks about designing APIs, defining HTTP endpoints, REST conventions, API versioning, request/response formats, URL structure, OpenAPI/Swagger specs, or reviewing an existing API contract for best practices.
---

# API Design

Use this when someone is designing a new HTTP API or reviewing an existing one.

## Resources and naming

URLs should describe resources, not actions. Use plural nouns for collections, singular for items.

- `GET /users` — list
- `GET /users/{id}` — single user
- `POST /users` — create
- `PATCH /users/{id}` — partial update
- `DELETE /users/{id}` — remove

Use kebab-case for multi-word path segments (`/access-tokens`, not `/access_tokens` or `/accessTokens`).

## Status codes

Pick the right code. Don't return 200 with an error body.

- `200` for successful reads and updates
- `201` for successful creates
- `204` for successful deletes
- `400` for client errors (bad input)
- `401` for missing or invalid auth
- `403` for authorized-but-forbidden
- `404` for not found
- `409` for conflict (e.g., duplicate)
- `422` for semantic validation errors
- `500` for unexpected server errors

## Pagination

For list endpoints, paginate. Return a structured response.

## Versioning

Version via URL prefix (`/v1/users`) or `Accept` header. Pick one and stick with it.

## Error responses

Errors should be useful. Include enough info for the client to act on.
