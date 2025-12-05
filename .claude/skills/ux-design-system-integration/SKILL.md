---
name: ux-design-system-integration
description: Map planned wireframes to an existing design system (shadcn), identify gaps, and propose new components or variants.
---

# Design System Integration (shadcn)

## Purpose

Use this Skill to:
- Map the planned UI (wireframes) to **shadcn** components.
- Identify where existing components cover the needs.
- Propose new components or variants to fill gaps.

## Inputs

The main agent should provide:
- Wireframe plan (screens and components)
- Any known constraints about using shadcn or Tailwind
- Links or references to which shadcn primitives are available (if specified)

## Process

1. **Component inventory from wireframes**
   - List all distinct UI elements needed:
     - Navigation elements
     - Forms and inputs
     - Lists, tables, cards
     - Modals, drawers, toasts
     - Miscellaneous controls (toggles, chips, steps, etc.)

2. **Map to shadcn components**
   - For each UI element, propose:
     - Matching shadcn component(s)
     - Any required props or variants at a high level
   - Note cases where multiple components might be combined.

3. **Identify gaps**
   - Mark elements that are:
     - Not clearly covered by shadcn
     - Require significant customization
   - For each gap, describe:
     - Desired behavior
     - Suggested API/props
     - Relationship to existing components (e.g., a variant of `Button` or `Card`).

4. **Usage guidelines**
   - Provide guidelines for:
     - Consistent spacing & layout primitives
     - Typography scale using design tokens
     - Common patterns (e.g., form layout, error display)

5. **Implementation notes**
   - Call out areas where design-system decisions might affect IA or flows.
   - Note any potential performance or complexity implications.

## Output

Produce a markdown **Design-System Integration Document** with:
- Component mapping table (Wireframe element → shadcn component(s) → Notes)
- List of proposed new components/variants
- Usage and consistency guidelines
- Implementation notes and open questions