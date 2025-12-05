---
description: Map UI designs to a design system (shadcn) for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Design System Integration

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Wireframes: !`cat workspaces/$ARGUMENTS/ui-ux/wireframes/wireframes.md 2>/dev/null || echo ""`
- Hi-Fi Specs: !`cat workspaces/$ARGUMENTS/ui-ux/high-fidelity/high-fidelity-specs.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `ui-ux-designer` agent mapping designs to a **Design System** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load wireframes and hi-fi specs for context

### Process

Use the **ux-design-system-integration** skill.

1. **Design System Selection**
   - Recommended: shadcn/ui (for React/Next.js)
   - Alternative options based on tech stack
   - Rationale for selection

2. **Component Mapping**
   - Map each custom component to design system equivalents
   - Identify components that need customization
   - List components that need to be built from scratch

3. **Theme Configuration**
   - Color theme mapping to design system variables
   - Typography mapping
   - Spacing and sizing adjustments

4. **Customization Requirements**
   - Component variants needed
   - Custom styles and overrides
   - New components to build

5. **Implementation Guide**
   - Installation instructions
   - Theme setup
   - Component usage examples

### Output

1. Create integration document at:
   `workspaces/{mvp-slug}/ui-ux/design-system/design-system-integration.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ Design system integration complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/ui-ux/design-system/
   
   Next step: /ui/plan-review {mvp-slug}
   ```
