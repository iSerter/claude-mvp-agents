---
description: Create API specification with OpenAPI/Swagger for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# API Specification

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Backend Plan: !`cat workspaces/$ARGUMENTS/code/backend/backend-plan.md 2>/dev/null || echo ""`
- PRD: !`cat workspaces/$ARGUMENTS/product/prd/prd.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are creating an **API Specification** in OpenAPI format for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load backend plan for context

### Process

1. **API Overview**
   - API title and description
   - Base URL structure
   - API versioning approach
   - Authentication methods

2. **Endpoints by Resource**
   For each resource/entity:
   - CRUD endpoints
   - Request/response schemas
   - Path parameters
   - Query parameters
   - Request body schemas
   - Response codes and bodies

3. **Authentication Endpoints**
   - Login/Register
   - Token refresh
   - Password reset
   - OAuth flows (if applicable)

4. **Schema Definitions**
   - Request DTOs
   - Response DTOs
   - Error response format
   - Pagination format

5. **Error Handling**
   - Standard error codes
   - Error response structure
   - Validation error format

6. **Rate Limiting**
   - Rate limit headers
   - Limit tiers

### Output Format

Create OpenAPI 3.0 YAML specification with:
- Complete path definitions
- Schema definitions with examples
- Security scheme definitions
- Tag groupings

### Output

1. Create OpenAPI spec at:
   `workspaces/{mvp-slug}/code/backend/openapi.yaml`

2. Create API documentation at:
   `workspaces/{mvp-slug}/code/backend/api-docs.md`

3. Display summary:
   ```
   ✅ API specification complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/code/backend/openapi.yaml
   
   Endpoints: {n} endpoints across {m} resources
   ```
