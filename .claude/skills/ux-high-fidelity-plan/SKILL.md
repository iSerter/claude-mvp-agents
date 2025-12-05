---
name: ux-high-fidelity-plan
description: Produce a high-fidelity UI design plan as detailed textual specifications for visual design and interaction, suitable for translation into Figma or code.
---

# High-Fidelity UI Design Plan

## Purpose

Use this Skill to create a **hi-fi design specification in text form**, which can be:
- Implemented in Figma by a designer
- Translated directly into HTML/CSS/React using shadcn and Tailwind

## Inputs

The main agent should provide:
- Approved wireframe plan
- Design-system integration document
- Any brand guidelines or visual direction (if available)

## Process

1. **Visual language definition**
   - Summarize:
     - Color usage and roles (primary, accent, background, feedback)
     - Typography scale (headings, body, captions)
     - Spacing system (e.g., 4/8px scale conceptually)

2. **Per-screen hi-fi spec**
   For each key screen:
   - Restate:
     - Screen purpose and primary user goal
   - Specify:
     - Layout details (relative sizing, alignment, responsive behavior)
     - Exact components (mapped to design-system elements)
     - Content hierarchy and emphasis
   - Describe interaction details:
     - Hover, focus, active, and disabled states
     - Transitions/animations (if relevant)
     - Error and validation messaging copy guidelines

3. **Component-level specs**
   - For complex components (e.g., tables, forms, composite cards):
     - Define:
       - Variants (default, compact, error, success, loading)
       - Key props (at a conceptual level)
       - Rules for truncation, wrapping, and responsiveness

4. **Accessibility & inclusivity**
   - Include guidelines for:
     - Target contrast levels
     - Focus indicators
     - Keyboard navigation patterns
     - Descriptive labels and helper text

5. **Handoff notes**
   - Add notes for:
     - Designers (Figma structure, naming suggestions)
     - Engineers (component reuse, layout approach)
   - Call out any elements that may need design exploration beyond this plan.

## Output

Produce a markdown **High-Fidelity Design Document** with:
- Global visual rules
- Per-screen hi-fi specs
- Component-level guidelines
- Accessibility guidance
- Handoff notes