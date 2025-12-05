---
description: Create information architecture and user flows for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Information Architecture & User Flows

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- PRD: !`cat workspaces/$ARGUMENTS/product/prd/prd.md 2>/dev/null || echo ""`
- Design Brief: !`cat workspaces/$ARGUMENTS/ui-ux/design-brief/design-brief.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `ui-ux-designer` agent creating **Information Architecture & User Flows** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load PRD and design brief for context

### Process

Use the **ux-information-architecture** skill.

1. **Site/App Structure**
   - Navigation hierarchy (tree structure)
   - Page/screen inventory
   - Content groupings and categories

2. **User Flows**
   - Primary user flows (3-5 key journeys)
   - Flow diagrams in Mermaid format
   - Decision points and branches
   - Entry and exit points

3. **Navigation Design**
   - Primary navigation structure
   - Secondary navigation
   - Breadcrumb strategy
   - Search and filtering approach

4. **Content Strategy**
   - Content types and templates
   - Content hierarchy
   - Labeling and taxonomy conventions

5. **Screen Inventory**
   - List all screens/pages needed
   - Screen purposes and relationships
   - Priority for wireframing

### Output

1. Create IA document at:
   `workspaces/{mvp-slug}/ui-ux/information-architecture/information-architecture.md`

2. Create user flows at:
   `workspaces/{mvp-slug}/ui-ux/information-architecture/user-flows.md`

3. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

4. Display summary:
   ```
   ✅ Information architecture complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/ui-ux/information-architecture/
   
   Next step: /ui/wireframes {mvp-slug}
   ```
