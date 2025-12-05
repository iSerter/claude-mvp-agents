---
name: po-prd-decomposition
description: Generate structured PRDs and decompose them into epics, features, user stories, and acceptance criteria for engineering teams. Use when preparing detailed requirements for implementation.
---

# PRD & Detailed Requirements (Epics → Stories)

## Purpose

Use this Skill to produce implementation-ready specifications:
- A clear Product Requirements Document (PRD)
- Epics and features tied to goals
- User stories with acceptance criteria
- Non-functional requirements and constraints

## Inputs

The main agent should provide:
- Product vision and goals for the scope in question
- Target users and primary problems being addressed
- Any constraints (technical, regulatory, timeline)
- Prioritized list of features or themes (if available)

If missing, derive them from earlier Skills and call out assumptions.

## Process

1. **PRD skeleton**
   - Create a PRD with sections:
     - Overview
     - Problem & context
     - Goals & non-goals
     - Target users & use cases
     - Scope (in / out)
     - Functional requirements
     - Non-functional requirements
     - Dependencies & risks
     - Success metrics

2. **Epics & features**
   - Group requirements into epics aligned with user workflows or outcomes.
   - For each epic:
     - Describe its purpose and success criteria.
     - List key features or capabilities.

3. **User stories**
   - For each feature, write user stories in the format:
     - "As a [user], I want to [action], so I can [outcome]."
   - Ensure coverage of core flows and important edge cases.

4. **Acceptance criteria**
   - Provide acceptance criteria per story using a clear format (e.g., bullet points or Gherkin-style Given/When/Then).
   - Include:
     - Happy path
     - Key edge cases
     - Error handling
     - Permission/role conditions where relevant

5. **Non-functional requirements**
   - Capture performance, security, reliability, and usability expectations at a practical level.
   - Avoid vague language; prefer concrete thresholds when possible.

6. **Handover readiness check**
   - Ensure output is:
     - Skimmable for stakeholders
     - Detailed enough for engineering and QA
     - Traceable back to goals and problems

7. **Outputs**

Produce markdown with:
- A full PRD
- A separate section for:
  - Epics
  - Features
  - User stories + acceptance criteria
- Optional: implementation notes or open questions to resolve with engineering/design

## Example

For a "Team workspaces" feature, you should:
- Define an epic around workspace creation & membership.
- Add stories for creating a workspace, inviting members, managing roles, and leaving a workspace.
- Include acceptance criteria for permission checks, error states, and basic audit needs.