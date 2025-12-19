---
description: Define product vision, value proposition, strategic pillars, and high-level roadmap for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Product Vision & Strategy

## Context

- MVP Slug: $ARGUMENTS
- Config Path: `workspaces/$ARGUMENTS/config.json`
- Discovery Path: `workspaces/$ARGUMENTS/product/discovery/discovery.md`
- Research Path: `workspaces/$ARGUMENTS/product/market-research/market-research.md`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are defining **Product Vision & Strategy** for the MVP workspace specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load discovery and research documents for context

### Process

Use the **po-vision-strategy** skill to create a compelling product direction.

1. **Problem Anchor**
   - Summarize the key problems and JTBD being addressed (from discovery)
   - Clarify which user segments are prioritized for v1
   - State the business goal (revenue, growth, strategic positioning)

2. **Product Vision**
   - Write a concise vision statement (1-3 sentences):
     - Who it's for
     - What change it creates
     - Why it matters
   - Add an "elevator pitch" (30-second narrative)

3. **Value Propositions**
   - Define 3-5 value propositions:
     - Target segment
     - Key outcome delivered
     - Why this product does it better than alternatives

4. **Strategic Pillars**
   - Define 3-5 strategic pillars that guide decisions
   - For each pillar:
     - What it means in practice
     - Example features/decisions it drives
     - What you will **NOT** do because of it

5. **High-Level Roadmap**
   - Propose phases:
     - **Phase 0 (Experiment):** Validate core hypothesis
     - **Phase 1 (Launch):** Minimum lovable product
     - **Phase 2 (Growth):** Expand and scale
   - For each phase: objectives, key bets, rough timeline

6. **Success Metrics**
   - Define 3-5 key metrics:
     - User value metrics (activation, retention, task success)
     - Business metrics (revenue, growth, engagement)
   - North star metric

### Output

1. Create strategy document at:
   `workspaces/{mvp-slug}/product/strategy/strategy.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.strategy` to "completed"
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ Strategy complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/product/strategy/strategy.md
   
   Next step: /po/prioritize {mvp-slug}
   ```
