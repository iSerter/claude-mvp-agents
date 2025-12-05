---
name: ux-ui-plan-review
description: Review previously generated UI/UX documents (brief, IA, flows, wireframes, design-system integration) and produce a critique & improvement plan.
---

# UI Plan Review

## Purpose

Use this Skill to critically review the existing UI/UX plan and ensure it is:
- Cohesive
- Feasible
- Aligned with user and business goals

## Inputs

The main agent should provide:
- Design brief
- IA & flows document
- Wireframe plan
- Design-system integration document
(Or as many of these as are available.)

## Process

1. **Coherence & alignment check**
   - Evaluate whether:
     - Flows support the stated objectives and tasks.
     - Screens and components match the IA.
     - Design-system mapping is realistic and complete.

2. **Usability & UX quality**
   - Identify potential issues:
     - Overly complex flows
     - Missing states or error handling
     - Ambiguous interactions
   - Highlight accessibility concerns.

3. **Design-system & implementation risk**
   - Check for:
     - Over-customization that may be hard to maintain.
     - Gaps in components that could block implementation.

4. **Prioritized feedback**
   - Group feedback into:
     - Must fix before implementation
     - Should improve
     - Nice to have / future iteration

5. **Revision recommendations**
   - For each major issue, propose:
     - What to change
     - Which document(s) should be updated (brief, IA, wireframes, etc.)

## Output

Produce a markdown **UI Plan Review** with:
- Summary of overall assessment
- Strengths
- Issues & risks (with severity)
- Detailed recommendations grouped by document
- Suggested next revision steps