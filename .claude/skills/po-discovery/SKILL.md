---
name: po-discovery
description: Discover and synthesize user problems, needs, and Jobs-To-Be-Done for a defined target audience and product idea. Use when understanding customers, their pains, and desired outcomes.
---

# Product Discovery (User Problems & JTBD)

## Purpose

Use this Skill when you need to deeply understand **who** the users are, **what problems** they experience, and **what outcomes** they care about for a given product idea or domain.

## Inputs

The main agent should provide, when available:
- Target audience (segments, roles, industries)
- High-level product idea or domain
- Business objective (e.g., acquisition, retention, expansion)
- Any existing hypotheses or constraints

If information is missing, infer reasonable assumptions and clearly label them as such.

## Process

1. **Clarify the context**
   - Restate the target audience, product idea, and business objective.
   - Identify key unknowns and assumptions.

2. **Generate user insights**
   - Identify primary user segments and roles.
   - For each segment, propose:
     - Typical goals and motivations
     - Primary pains and friction points
     - Triggers that make the problem urgent

3. **Jobs-To-Be-Done (JTBD)**
   - For each key segment, express 3–7 core jobs in the JTBD format:
     - "When [situation], I want to [motivation], so I can [expected outcome]."

4. **Problem statements**
   - Synthesize 5–10 concise problem statements:
     - Format: "Users in [segment] struggle with [problem], which leads to [impact]."
   - Highlight which problems seem most critical or urgent.

5. **Evidence & confidence**
   - For each major problem:
     - Assign a rough confidence level (low / medium / high).
     - Note what kind of evidence would increase confidence (e.g., interviews, analytics, surveys).

6. **Outputs**

Produce a markdown report with sections:
- Target audience & context
- User segments & personas (lightweight)
- Jobs-To-Be-Done
- Problem statements
- Assumptions & open questions
- Recommended validation next steps

## Examples

**Example use case**

> "We’re exploring a B2B SaaS product for small e-commerce brands to improve post-purchase experience."

You should:
- Identify key user segments (e.g., founder, ops manager, customer support).
- List JTBD for each segment.
- List pain points around tracking, communication, returns, and loyalty.
- Propose problem statements and validation experiments (e.g., 10 customer interviews, pull support ticket data).