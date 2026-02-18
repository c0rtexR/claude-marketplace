# {Feature Name} — API Specification

## Base Path

`/api/v1/{resource}`

## Authentication

{Authentication method: Bearer token, API key, session, etc.}

## Endpoints

| Method | Path | Description | Auth |
|--------|------|-------------|------|
| GET | `/{resource}` | List items | Yes |
| GET | `/{resource}/:id` | Get item | Yes |
| POST | `/{resource}` | Create item | Yes |
| PUT | `/{resource}/:id` | Update item | Yes |
| DELETE | `/{resource}/:id` | Delete item | Yes |

## Endpoint Details

### GET `/{resource}`

**Query Parameters:**

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| page | int | No | 1 | Page number |
| limit | int | No | 20 | Items per page (max 100) |
| sort | string | No | created_at | Sort field |
| order | string | No | desc | asc / desc |

**Response 200:**

```json
{
  "data": [{ "id": "string", "created_at": "ISO8601" }],
  "pagination": { "page": 1, "limit": 20, "total": 100, "total_pages": 5 }
}
```

### GET `/{resource}/:id`

**Response 200:**

```json
{ "id": "string", "created_at": "ISO8601" }
```

**Response 404:**

```json
{ "error": "not_found", "message": "Resource not found" }
```

### POST `/{resource}`

**Request Body:**

```json
{ "field": "value" }
```

**Response 201:** Created resource object

**Response 400:**

```json
{
  "error": "validation_error",
  "message": "Validation failed",
  "details": [{ "field": "name", "message": "Required" }]
}
```

## Error Responses

| Code | Error Key | Description |
|------|-----------|-------------|
| 400 | validation_error | Validation failed |
| 401 | unauthorized | Missing/invalid auth |
| 403 | forbidden | Insufficient permissions |
| 404 | not_found | Resource doesn't exist |
| 409 | conflict | Resource already exists |
| 500 | internal_error | Server error |

## UI/API Contract

| UI Element | API Field | Notes |
|------------|-----------|-------|
| {component} | `response.field` | {mapping} |
