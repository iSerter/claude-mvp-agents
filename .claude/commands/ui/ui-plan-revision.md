---
description: Revise UI plan based on review feedback for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# UI Plan Revision

## Context

- MVP Slug: $ARGUMENTS
- Config Path: `workspaces/$ARGUMENTS/config.json`
- Review Path: `workspaces/$ARGUMENTS/ui-ux/reviews/ui-plan-review.md`
- Wireframes Path: `workspaces/$ARGUMENTS/ui-ux/wireframes/wireframes.md`
- Hi-Fi Specs Path: `workspaces/$ARGUMENTS/ui-ux/high-fidelity/high-fidelity-specs.md`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `ui-ux-designer` agent performing a **UI Plan Revision** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load review document and existing designs

### Process

Use the **ux-ui-plan-revision** skill.

1. **Review Analysis**
   - Parse critical issues from review
   - Parse important improvements
   - Prioritize changes by impact

2. **Revision Plan**
   - List specific changes to make
   - Affected screens/components
   - Estimated effort per change

3. **Execute Revisions**
   - Update wireframe specifications
   - Update hi-fi specifications
   - Update component specifications
   - Update interaction patterns

4. **Change Log**
   - Document all changes made
   - Before/after comparisons
   - Rationale for each change

5. **Re-validation**
   - Confirm critical issues addressed
   - Verify no regressions introduced
   - Note any remaining concerns

### Output

1. Create revision log at:
   `workspaces/{mvp-slug}/ui-ux/reviews/revision-log.md`

2. Update existing design documents in place

3. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

4. Display summary:
   ```
   ✅ UI plan revision complete for {mvp-name}
   📁 Revision log: workspaces/{mvp-slug}/ui-ux/reviews/revision-log.md
   
   Changes made:
   - {n} critical issues fixed
   - {n} improvements applied
   
   Design phase complete!
   ```
