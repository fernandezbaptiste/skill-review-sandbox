---
name: api-design
description: Designs RESTful HTTP APIs by structuring endpoints, selecting appropriate status codes, and defining request/response schemas. Use when the user asks about API design, REST endpoints, route structure, URL naming conventions, OpenAPI/Swagger specs, pagination patterns, versioning strategies, or error response formats.
---

# API Design

Use this for HTTP REST API design.

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
