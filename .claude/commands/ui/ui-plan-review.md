---
description: Review and validate the UI plan for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# UI Plan Review

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Design Brief: !`cat workspaces/$ARGUMENTS/ui-ux/design-brief/design-brief.md 2>/dev/null || echo ""`
- IA: !`cat workspaces/$ARGUMENTS/ui-ux/information-architecture/information-architecture.md 2>/dev/null || echo ""`
- Wireframes: !`cat workspaces/$ARGUMENTS/ui-ux/wireframes/wireframes.md 2>/dev/null || echo ""`
- Hi-Fi Specs: !`cat workspaces/$ARGUMENTS/ui-ux/high-fidelity/high-fidelity-specs.md 2>/dev/null || echo ""`
- PRD: !`cat workspaces/$ARGUMENTS/product/prd/prd.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `ui-ux-designer` agent performing a **UI Plan Review** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load all design artifacts for review

### Process

Use the **ux-ui-plan-review** skill.

1. **Completeness Check**
   - Verify all PRD features have corresponding UI
   - Check all user flows have screens designed
   - Identify missing states or edge cases

2. **Consistency Review**
   - Visual consistency across screens
   - Interaction pattern consistency
   - Terminology and labeling consistency

3. **Usability Assessment**
   - Navigation clarity
   - Information hierarchy
   - Cognitive load assessment
   - Error prevention and recovery

4. **Accessibility Review**
   - Color contrast compliance
   - Touch target sizes
   - Screen reader considerations
   - Keyboard navigation

5. **Technical Feasibility**
   - Implementation complexity
   - Performance considerations
   - Responsive design completeness

6. **Recommendations**
   - Critical issues (must fix)
   - Important improvements (should fix)
   - Nice-to-have enhancements (could fix)

### Output

1. Create review document at:
   `workspaces/{mvp-slug}/ui-ux/reviews/ui-plan-review.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ UI plan review complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/ui-ux/reviews/ui-plan-review.md
   
   Next step: /ui/plan-revision {mvp-slug} (if issues found)
   ```
