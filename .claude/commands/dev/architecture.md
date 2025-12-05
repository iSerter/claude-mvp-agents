---
description: Create technical architecture document for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Technical Architecture

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- PRD: !`cat workspaces/$ARGUMENTS/product/prd/prd.md 2>/dev/null || echo ""`
- Design Specs: !`cat workspaces/$ARGUMENTS/ui-ux/high-fidelity/high-fidelity-specs.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are creating a **Technical Architecture Document** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load PRD and design specs for requirements

### Process

1. **Architecture Overview**
   - High-level system architecture diagram (Mermaid)
   - Technology stack recommendations
   - Key architectural decisions and rationale

2. **Component Architecture**
   - Frontend architecture (Next.js recommended)
   - Backend architecture (Nest.js recommended)
   - Mobile architecture (if applicable)
   - Database architecture

3. **Data Architecture**
   - Data models and relationships (ERD in Mermaid)
   - Database selection and justification
   - Data flow diagrams
   - Caching strategy

4. **API Architecture**
   - API design approach (REST/GraphQL)
   - Authentication/authorization flow
   - Rate limiting and throttling
   - API versioning strategy

5. **Infrastructure**
   - Deployment architecture
   - Environment strategy (dev/staging/prod)
   - CI/CD pipeline overview
   - Monitoring and logging approach

6. **Security Architecture**
   - Authentication mechanisms
   - Authorization model
   - Data protection measures
   - Security best practices

7. **Scalability Considerations**
   - Horizontal vs vertical scaling
   - Bottleneck identification
   - Performance targets

8. **Technical Risks**
   - Identified risks
   - Mitigation strategies
   - Technology dependencies

### Output

1. Create architecture document at:
   `workspaces/{mvp-slug}/code/architecture/architecture.md`

2. Create ERD diagram at:
   `workspaces/{mvp-slug}/code/architecture/erd.md`

3. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.development` to "in-progress"
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

4. Display summary:
   ```
   ✅ Architecture complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/code/architecture/
   
   Next steps:
   - Backend: /dev/backend {mvp-slug}
   - Frontend: /dev/frontend {mvp-slug}
   - Mobile: /dev/mobile {mvp-slug}
   ```
