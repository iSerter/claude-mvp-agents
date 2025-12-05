---
name: po-prioritization
description: Score and prioritize opportunities, problems, or features using frameworks like RICE, MoSCoW, and value-risk analysis. Use when deciding what to build first and how to sequence a roadmap.
---

# Opportunity Prioritization & Roadmapping

## Purpose

Use this Skill to:
- Turn a list of opportunities / features / problems into a prioritized backlog
- Provide rational, transparent scoring
- Suggest a sensible sequence for delivery (roadmap)

## Inputs

The main agent should provide:
- A list of opportunities, problems, or features to evaluate
- Any constraints (e.g., team capacity, deadlines, technical dependencies)
- Preferred framework if specified (e.g., RICE, MoSCoW)

If not provided, choose a simple, transparent framework (RICE or value vs effort).

## Process

1. **Normalize the items**
   - Rewrite each item into a clear, action-oriented statement.
   - Group related items into themes where helpful.

2. **Select frameworks**
   - Prefer RICE (Reach, Impact, Confidence, Effort) for product features.
   - Optionally add:
     - MoSCoW (Must / Should / Could / Won’t)
     - Risk level (low / medium / high)

3. **Score each item**
   - For each item, estimate:
     - Reach: how many users are affected in a given period
     - Impact: qualitative impact per user
     - Confidence: how sure we are about reach/impact
     - Effort: rough size (e.g., S/M/L or story points / weeks)
   - Be explicit that these are estimates and note major assumptions.

4. **Compute prioritization**
   - Calculate RICE score or equivalent.
   - Place items into a table with columns:
     - Item
     - Theme
     - Reach / Impact / Confidence / Effort
     - RICE score
     - Risk level
     - Priority bucket (Now / Next / Later)

5. **Propose sequencing**
   - Suggest a phased roadmap:
     - Phase 1: high-value, lower-risk, foundational items
     - Phase 2: higher complexity or expansion bets
     - Phase 3: nice-to-haves or experiments
   - Call out dependencies explicitly.

6. **Outputs**

Produce markdown with:
- Short explanation of the chosen framework
- Prioritization table
- Suggested "Now / Next / Later" grouping or phased roadmap
- Notes on key assumptions, risks, and validation needs

## Example

Given 12 features for an MVP:
- Normalize them
- Score using RICE
- Propose 4–5 as core MVP, 4–5 as "Next", and the rest as "Later" with clear rationale.