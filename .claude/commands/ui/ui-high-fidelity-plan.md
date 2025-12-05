---
description: Create high-fidelity UI design specifications for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# High-Fidelity UI Design Plan

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Wireframes: !`cat workspaces/$ARGUMENTS/ui-ux/wireframes/wireframes.md 2>/dev/null || echo ""`
- Design Brief: !`cat workspaces/$ARGUMENTS/ui-ux/design-brief/design-brief.md 2>/dev/null || echo ""`
- Brand: !`cat workspaces/$ARGUMENTS/graphics/brand-assets/brand-guidelines.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `ui-ux-designer` agent creating **High-Fidelity UI Design Specifications** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load wireframes and brand guidelines for context

### Process

Use the **ux-high-fidelity-plan** skill.

1. **Visual Design System**
   - Color palette application (primary, secondary, semantic)
   - Typography scale and hierarchy
   - Spacing system (8px grid)
   - Iconography style and sources
   - Imagery guidelines (photos, illustrations)

2. **Component Specifications**
   For each UI component:
   - Visual design details
   - All states (default, hover, active, focus, disabled, error)
   - Responsive behavior
   - Animation/transition specs
   - Accessibility considerations

3. **Screen Designs**
   For each key screen:
   - Detailed visual specifications
   - Component placement with exact spacing
   - Content specifications
   - Responsive breakpoints (mobile-first)

4. **Interaction Design**
   - Micro-interactions and animations
   - Transitions between screens
   - Loading states and skeletons
   - Error states and recovery
   - Success confirmations

5. **Design Tokens**
   Export-ready design tokens:
   - Colors (hex, RGB, HSL)
   - Typography (font families, sizes, weights)
   - Spacing scale
   - Shadows and elevation
   - Border radius
   - Transitions/animations

### Output

1. Create high-fidelity specs at:
   `workspaces/{mvp-slug}/ui-ux/high-fidelity/high-fidelity-specs.md`

2. Create design tokens at:
   `workspaces/{mvp-slug}/ui-ux/high-fidelity/design-tokens.json`

3. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.design` to "completed"
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

4. Display summary:
   ```
   ✅ High-fidelity design complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/ui-ux/high-fidelity/
   
   Design phase complete! Next steps:
   - Branding: /gd/logo-briefs {mvp-slug}
   - Development: /dev/architecture {mvp-slug}
   ```
