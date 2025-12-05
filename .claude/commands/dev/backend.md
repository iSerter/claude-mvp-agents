---
description: Create backend implementation plan using Nest.js for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Backend Implementation Plan (Nest.js)

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Architecture: !`cat workspaces/$ARGUMENTS/code/architecture/architecture.md 2>/dev/null || echo ""`
- PRD: !`cat workspaces/$ARGUMENTS/product/prd/prd.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `nestjs-developer` agent creating a **Backend Implementation Plan** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load architecture document for context

### Process

1. **Project Structure**
   ```
   src/
   ├── modules/           # Feature modules
   ├── common/            # Shared utilities
   ├── config/            # Configuration
   ├── database/          # Database setup
   └── main.ts
   ```

2. **Module Breakdown**
   For each feature module:
   - Module name and purpose
   - Controllers and routes
   - Services and business logic
   - Entities/DTOs
   - Dependencies

3. **Database Design**
   - Entity definitions with TypeORM/Prisma
   - Relationships and constraints
   - Migrations strategy
   - Seeding approach

4. **API Endpoints**
   - RESTful endpoint specifications
   - Request/Response DTOs
   - Validation rules
   - OpenAPI/Swagger documentation

5. **Authentication & Authorization**
   - Auth module structure
   - JWT configuration
   - Guards and decorators
   - Role-based access control

6. **Error Handling**
   - Exception filters
   - Error response format
   - Logging strategy

7. **Testing Strategy**
   - Unit test structure
   - Integration test approach
   - E2E test setup

8. **Implementation Tasks**
   - Task breakdown with estimates
   - Dependencies between tasks
   - Suggested sprint allocation

### Output

1. Create backend plan at:
   `workspaces/{mvp-slug}/code/backend/backend-plan.md`

2. Create API spec at:
   `workspaces/{mvp-slug}/code/backend/api-spec.md`

3. Create database schema at:
   `workspaces/{mvp-slug}/code/backend/database-schema.md`

4. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

5. Display summary:
   ```
   ✅ Backend plan complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/code/backend/
   
   Tech stack: Nest.js + TypeScript + PostgreSQL
   
   Next: /dev/frontend {mvp-slug}
   ```
