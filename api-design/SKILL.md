---
name: api-design
description: Helps with API design.
---

# API Design

Use this for HTTP API design.

## URLs

Use nouns. Plural for collections, singular for items.

- GET /users
- GET /users/{id}
- POST /users
- PATCH /users/{id}
- DELETE /users/{id}

## Status codes

- 200 OK
- 201 Created
- 204 No Content
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 422 Validation
- 500 Server Error

## Pagination

Paginate.

## Versioning

Use a version prefix.

## Errors

Return useful errors.
