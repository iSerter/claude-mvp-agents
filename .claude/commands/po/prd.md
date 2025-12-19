---
description: Generate a complete PRD with epics, features, user stories, and acceptance criteria for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Product Requirements Document (PRD)

## Context

- MVP Slug: $ARGUMENTS
- Config Path: `workspaces/$ARGUMENTS/config.json`
- Discovery Path: `workspaces/$ARGUMENTS/product/discovery/discovery.md`
- Strategy Path: `workspaces/$ARGUMENTS/product/strategy/strategy.md`
- Roadmap Path: `workspaces/$ARGUMENTS/product/roadmap/roadmap.md`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are generating a **Product Requirements Document (PRD)** for the MVP workspace specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load all prior artifacts for context

### Process

Use the **po-prd-decomposition** skill to create implementation-ready specifications.

1. **PRD Header**
   - Title, version, date, author
   - Status (Draft/Review/Approved)
   - Stakeholders and approvers

2. **Overview & Context**
   - Problem statement (1-2 paragraphs)
   - Target users
   - Business objectives
   - Success metrics

3. **Scope**
   - **In Scope:** What this PRD covers
   - **Out of Scope:** What is explicitly excluded
   - **Assumptions:** Key assumptions being made

4. **Epics**
   - Group features into 3-6 epics aligned with user workflows
   - For each epic:
     - Name and description
     - User value / business value
     - Success criteria

5. **Features & User Stories**
   - For each epic, list features
   - For each feature, write user stories:
     - "As a [user], I want to [action], so I can [outcome]."
   - Estimate complexity (S/M/L/XL)

6. **Acceptance Criteria**
   - For each user story, provide acceptance criteria:
     - Use bullet points or Given/When/Then format
     - Cover happy path, edge cases, error handling
     - Include permission/role conditions where relevant

7. **Non-Functional Requirements**
   - Performance (response times, throughput)
   - Security (authentication, authorization, data protection)
   - Reliability (uptime, error handling)
   - Usability (accessibility, mobile support)

8. **Dependencies & Risks**
   - Technical dependencies
   - External dependencies (APIs, services)
   - Key risks and mitigation strategies

9. **Open Questions**
   - List unresolved questions for engineering/design/stakeholders

### Output

1. Create PRD document at:
   `workspaces/{mvp-slug}/product/prd/prd.md`

2. Create individual epic files at:
   `workspaces/{mvp-slug}/product/prd/epics/epic-{n}-{slug}.md`

3. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.prd` to "completed"
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

4. Display summary:
   ```
   ✅ PRD complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/product/prd/prd.md
   📁 Epics: workspaces/{mvp-slug}/product/prd/epics/
   
   Next steps:
   - Design: /ui/design-brief {mvp-slug}
   - Architecture: /dev/architecture {mvp-slug}
   ```
