---
description: Create database schema and migration plan for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Database Schema Design

## Context

- MVP Slug: $ARGUMENTS
- Config Path: `workspaces/$ARGUMENTS/config.json`
- Architecture Path: `workspaces/$ARGUMENTS/code/architecture/architecture.md`
- PRD Path: `workspaces/$ARGUMENTS/product/prd/prd.md`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are creating a **Database Schema Design** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load architecture and PRD for context

### Process

1. **Database Selection**
   - Recommended database (PostgreSQL, MongoDB, etc.)
   - Justification for selection
   - ORM recommendation (Prisma, TypeORM, Drizzle)

2. **Entity Relationship Diagram**
   - Complete ERD in Mermaid format
   - Table relationships
   - Cardinality notation

3. **Table Definitions**
   For each table/collection:
   - Table name and purpose
   - Column definitions (name, type, constraints)
   - Primary keys
   - Foreign keys
   - Indexes
   - Default values

4. **Relationships**
   - One-to-one relationships
   - One-to-many relationships
   - Many-to-many relationships (junction tables)

5. **Schema Definition**
   - Prisma schema OR
   - TypeORM entities OR
   - Raw SQL DDL

6. **Migration Strategy**
   - Initial migration
   - Versioning approach
   - Rollback strategy

7. **Seeding**
   - Seed data requirements
   - Test data generation

8. **Performance Considerations**
   - Index recommendations
   - Query optimization hints
   - Partitioning (if needed)

### Output

1. Create database schema at:
   `workspaces/{mvp-slug}/code/backend/database/schema.prisma` (or equivalent)

2. Create ERD at:
   `workspaces/{mvp-slug}/code/backend/database/erd.md`

3. Create seed file at:
   `workspaces/{mvp-slug}/code/backend/database/seed.md`

4. Display summary:
   ```
   ✅ Database schema complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/code/backend/database/
   
   Tables: {n} tables
   Database: PostgreSQL + Prisma
   ```
