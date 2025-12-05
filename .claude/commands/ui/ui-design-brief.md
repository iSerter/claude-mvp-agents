---
description: Create a UI/UX design brief from PRD for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# UI/UX Design Brief

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- PRD: !`cat workspaces/$ARGUMENTS/product/prd/prd.md 2>/dev/null || echo ""`
- Strategy: !`cat workspaces/$ARGUMENTS/product/strategy/strategy.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `ui-ux-designer` agent creating a **Design Brief** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. PRD should ideally be completed first

### Process

Use the **ux-design-brief** skill.

1. **Design Brief Overview**
   - Project summary (from PRD)
   - Design goals and objectives
   - Target users (from PRD/discovery)
   - Brand personality guidelines

2. **User Experience Goals**
   - Primary UX goals (3-5)
   - Emotional design targets
   - Accessibility requirements (WCAG level)

3. **Design Constraints**
   - Technical constraints (frameworks, platforms)
   - Business constraints (timeline, resources)
   - Brand constraints

4. **Inspiration & References**
   - Competitive design analysis
   - Design inspiration sources
   - Anti-patterns to avoid

5. **Deliverables**
   - List of expected design deliverables
   - Priority order
   - Dependencies

### Output

1. Create design brief at:
   `workspaces/{mvp-slug}/ui-ux/design-brief/design-brief.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.design` to "in-progress"
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ Design brief complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/ui-ux/design-brief/design-brief.md
   
   Next step: /ui/information-architecture {mvp-slug}
   ```
