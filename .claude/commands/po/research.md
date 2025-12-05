---
description: Analyze competitors and market landscape for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Market & Competitor Research

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Discovery: !`cat workspaces/$ARGUMENTS/product/discovery/discovery.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are performing **Market & Competitor Research** for the MVP workspace specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load discovery document if available for context

### Process

Use the **po-market-research** skill to analyze the competitive landscape.

1. **Define the Market Scope**
   - Restate the problem space and target market (from config brief and discovery)
   - Identify what types of competition matter:
     - Direct competitors (same problem, same approach)
     - Indirect competitors (same problem, different approach)
     - Substitutes (DIY, spreadsheets, "do nothing")

2. **Competitor Overview**
   - List 5-10 relevant competitors or alternatives
   - For each, capture:
     - Name and URL
     - Target segment
     - Core value proposition
     - Key features
     - Pricing model (free tier, per-seat, usage-based, etc.)
     - Strengths & weaknesses

3. **Feature Comparison Matrix**
   - Create a comparison table for top 3-5 competitors
   - Rows: key capabilities that matter to users
   - Columns: competitors + "Our Product"
   - Use ✅/⚠️/❌ or similar visual indicators

4. **Market Gaps & Opportunities**
   - Identify:
     - Underserved user segments
     - Poorly solved jobs or workflows
     - Over-engineered solutions (opportunity for simplicity)
   - Propose 3-5 differentiation angles

5. **Risks & Barriers**
   - List market risks (incumbents, switching costs, regulatory)
   - Suggest mitigation strategies

### Output

1. Create research document at:
   `workspaces/{mvp-slug}/product/market-research/market-research.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.research` to "completed"
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ Market research complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/product/market-research/market-research.md
   
   Next step: /po/strategy {mvp-slug}
   ```
