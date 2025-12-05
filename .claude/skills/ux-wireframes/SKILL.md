---
name: ux-wireframes
description: Plan low-fidelity wireframes for key screens in the user flows. Use after IA & flows are defined.
---

# Wireframe Planning (Key Screens)

## Purpose

Use this Skill to define **wireframes in text form**:
- What appears on each key screen
- Layout regions and content hierarchy
- Primary actions and states

These are not pixel-perfect, but detailed enough for a designer or engineer to build from.

## Inputs

The main agent should provide:
- Information architecture & flows
- Design brief
- Any existing layout constraints or patterns (e.g., app shell, sidebar)

## Process

1. **Identify key screens**
   - For each major flow, list the key screens and states that need wireframes.
   - Name them clearly (e.g., "Dashboard - Empty", "Project Detail - Populated").

2. **Define layout regions**
   For each screen, describe:
   - Global layout (e.g., header, sidebar, content area)
   - Main content sections
   - Key components (tables, forms, cards, lists)

3. **Content & interactions**
   - For each section:
     - Describe what data is shown (at a field/column level if helpful).
     - Primary actions (buttons, links, CTAs).
     - Secondary actions and affordances (sorting, filters, menus).

4. **States & feedback**
   - Describe how the screen looks/behaves in:
     - Loading
     - Empty
     - Error
     - Success
   - Include inline validation and feedback where relevant.

5. **Accessibility & usability notes**
   - Call out important accessibility considerations (keyboard navigation, focus order, contrast-sensitive elements).
   - Note any UX risks (e.g., complexity, information overload).

## Output

Produce markdown with a section per screen:
- Screen name
- Purpose
- Layout description
- Content & actions
- States
- Notes & considerations

Use bullet points and structured lists, not ASCII art or diagrams.