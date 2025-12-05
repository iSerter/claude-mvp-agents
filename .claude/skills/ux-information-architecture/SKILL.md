---
name: ux-information-architecture
description: Define information architecture and user flows for the product or feature. Use after a design brief is created.
---

# Information Architecture & Flows

## Purpose

Use this Skill to produce:
- A clear **information architecture (IA)** for the product or feature.
- **User flows** that describe step-by-step how users accomplish key tasks.

## Inputs

The main agent should provide:
- Design brief or PRD
- List of core user tasks / scenarios
- Any technical or navigation constraints (e.g., existing app shell, routing model)

## Process

1. **Identify key entities and objects**
   - List core domain objects (e.g., projects, tasks, teams).
   - Identify relationships between them (one-to-many, parent-child).

2. **Define navigation structure**
   - Propose:
     - Global navigation (top-level sections)
     - Secondary navigation (subsections)
     - Contextual navigation (within flows)
   - Represent as a simple hierarchical outline or tree.

3. **User flows**
   - For each core user task:
     - Define the start trigger.
     - Enumerate steps as a flow:
       - Step number
       - User action
       - System response
     - Include branches for key decision points and error paths.

4. **States & variants**
   - For important screens or objects, list key states:
     - Empty state
     - Loading state
     - Error state
     - Populated / success state

5. **Consideration of edge cases**
   - Identify edge cases that significantly impact IA or flows (e.g., permissions, missing data, timeouts).

## Output

Produce markdown with:
- Information architecture as a hierarchical list / tree
- A section per flow:
  - Flow name
  - Goal
  - Preconditions
  - Step-by-step flow (with branches)
- Notes on states and edge cases