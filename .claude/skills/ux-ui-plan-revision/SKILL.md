---
name: ux-ui-plan-revision
description: Revise the UI/UX plan documents based on a review document, applying prioritized feedback.
---

# UI Plan Revision

## Purpose

Use this Skill to update the UI/UX plan:
- Apply feedback from a review document.
- Produce revised versions of the design brief, IA/flows, wireframes, and design-system integration as needed.

## Inputs

The main agent should provide:
- Original documents (or excerpts) to be revised
- UI Plan Review document, including prioritized feedback

## Process

1. **Identify required changes**
   - Extract "must fix" and "should improve" items from the review.
   - Map each item to the specific document/section it affects.

2. **Revise documents**
   - For each impacted document:
     - Update sections to address the feedback.
     - Maintain clarity and structure.
     - Ensure changes stay aligned with goals and constraints.

3. **Call out deltas**
   - For each revised document, briefly summarize what changed and why.
   - Note any feedback that was intentionally not applied and why.

4. **Consistency check**
   - Ensure that:
     - Design brief ↔ IA ↔ wireframes ↔ design-system mapping remain consistent.
     - New assumptions or constraints are explicitly stated.

## Output

Produce markdown that includes:
- Summary of applied changes
- Revised sections of each document (or full revised documents, if scope is small enough)
- Any remaining open questions or tradeoffs