---
description: Create wireframe specifications for key screens of an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Wireframe Specifications

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- PRD: !`cat workspaces/$ARGUMENTS/product/prd/prd.md 2>/dev/null || echo ""`
- IA: !`cat workspaces/$ARGUMENTS/ui-ux/information-architecture/information-architecture.md 2>/dev/null || echo ""`
- User Flows: !`cat workspaces/$ARGUMENTS/ui-ux/information-architecture/user-flows.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `ui-ux-designer` agent creating **Wireframe Specifications** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load IA and user flows for context

### Process

Use the **ux-wireframes** skill.

1. **Screen Inventory**
   - List all screens from IA
   - Priority order for wireframing
   - Screen relationships and navigation

2. **Wireframe Specs per Screen**
   For each key screen:
   - Screen name and purpose
   - Layout description (ASCII diagram or detailed spec)
   - Component inventory
   - Content placeholders
   - Interaction notes
   - Responsive considerations (mobile/tablet/desktop)

3. **Component Library**
   - Reusable components identified
   - Component specifications
   - State variations (default, hover, active, disabled, error)

4. **Interaction Patterns**
   - Navigation patterns
   - Form patterns
   - Feedback patterns (success, error, loading)
   - Empty states
   - Error handling patterns

### Output

1. Create wireframe overview at:
   `workspaces/{mvp-slug}/ui-ux/wireframes/wireframes.md`

2. Create individual screen specs at:
   `workspaces/{mvp-slug}/ui-ux/wireframes/screens/{screen-slug}.md`

3. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

4. Display summary:
   ```
   ✅ Wireframes complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/ui-ux/wireframes/
   
   Next step: /ui/high-fidelity-plan {mvp-slug}
   ```
