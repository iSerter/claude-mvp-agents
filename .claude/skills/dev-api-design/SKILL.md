---
name: dev-api-design
description: Design RESTful APIs and GraphQL schemas with proper conventions, documentation, and type safety. Use when planning API contracts, endpoints, or service interfaces.
---

# API Design

## Purpose

Use this Skill to design robust, well-documented APIs:
- RESTful endpoint design following best practices
- GraphQL schema design (if applicable)
- Request/response contracts with validation
- OpenAPI/Swagger specification generation

## Inputs

The main agent should provide:
- Feature requirements or user stories
- Data models and relationships
- Authentication/authorization requirements
- Performance expectations

## Process

### 1. Resource Identification

Identify API resources from requirements:
- Map business entities to resources
- Define resource relationships
- Establish resource hierarchies

```
Example resources for a project management app:
- /users
- /projects
- /projects/{id}/tasks
- /projects/{id}/members
```

### 2. Endpoint Design (REST)

For each resource, define CRUD operations:

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | /resources | List with pagination/filtering | Required |
| GET | /resources/:id | Get single resource | Required |
| POST | /resources | Create new resource | Required |
| PATCH | /resources/:id | Partial update | Required |
| DELETE | /resources/:id | Soft/hard delete | Required |

### 3. Request/Response DTOs

Define typed contracts:

```typescript
// Request DTO
interface CreateProjectRequest {
  name: string;           // Required, 3-100 chars
  description?: string;   // Optional, max 1000 chars
  visibility: 'public' | 'private';
  teamId?: string;        // UUID, optional
}

// Response DTO
interface ProjectResponse {
  id: string;
  name: string;
  description: string | null;
  visibility: 'public' | 'private';
  owner: UserSummary;
  createdAt: string;      // ISO 8601
  updatedAt: string;
}

// List Response (paginated)
interface PaginatedResponse<T> {
  data: T[];
  meta: {
    total: number;
    page: number;
    pageSize: number;
    totalPages: number;
  };
}
```

### 4. Validation Rules

Document validation for each field:
- Type constraints
- Length/size limits
- Format requirements (email, URL, etc.)
- Business rules

### 5. Error Handling

Define consistent error responses:

```typescript
interface ErrorResponse {
  error: {
    code: string;         // Machine-readable code
    message: string;      // Human-readable message
    details?: object;     // Validation errors, etc.
    requestId: string;    // For debugging
  };
}

// HTTP Status Code Usage
// 400 - Bad Request (validation errors)
// 401 - Unauthorized (no/invalid auth)
// 403 - Forbidden (no permission)
// 404 - Not Found
// 409 - Conflict (duplicate, etc.)
// 422 - Unprocessable Entity (business rule)
// 429 - Too Many Requests
// 500 - Internal Server Error
```

### 6. Query Parameters

Standardize query conventions:

**Pagination:**
- `?page=1&pageSize=20` or `?offset=0&limit=20`
- `?cursor=abc123` for cursor-based

**Filtering:**
- `?status=active`
- `?createdAfter=2024-01-01`
- `?search=keyword`

**Sorting:**
- `?sort=createdAt:desc`
- `?sort=-createdAt,name` (- for desc)

**Field Selection:**
- `?fields=id,name,status`
- `?include=owner,tasks`

### 7. Authentication & Authorization

Document auth requirements:
- Which endpoints require authentication
- Permission requirements per endpoint
- Rate limiting per auth level

### 8. Versioning Strategy

Choose and document versioning approach:
- URL versioning: `/api/v1/resources`
- Header versioning: `Accept: application/vnd.api+json;version=1`
- Query param: `?version=1`

### 9. OpenAPI Specification

Generate OpenAPI 3.0 spec:

```yaml
openapi: 3.0.3
info:
  title: Project API
  version: 1.0.0
paths:
  /projects:
    get:
      summary: List projects
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ProjectList'
```

## Output Format

Produce documentation with:
- Resource overview table
- Endpoint specifications (method, path, description)
- Request/response examples (JSON)
- Validation rules table
- Error codes reference
- OpenAPI specification file

## Best Practices

- **Use nouns, not verbs** for resource names
- **Be consistent** with naming conventions
- **Version from day one**
- **Document everything** including edge cases
- **Design for idempotency** where possible
- **Include request IDs** for debugging
