---
description: Create logo design brief with creative directions for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Logo Design Brief

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Strategy: !`cat workspaces/$ARGUMENTS/product/strategy/strategy.md 2>/dev/null || echo ""`
- Design Brief: !`cat workspaces/$ARGUMENTS/ui-ux/design-brief/design-brief.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `graphic-designer` agent creating a **Logo Design Brief** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load strategy and design brief for brand context

### Process

Use the **gd-logo-briefs** skill.

1. **Brand Context**
   - Brand name and tagline
   - Brand personality (from strategy)
   - Target audience
   - Industry context and competitors

2. **Logo Requirements**
   - Logo type preferences (wordmark, symbol, combination mark)
   - Must-have elements
   - Must-avoid elements
   - Technical requirements (sizes, formats, backgrounds)

3. **Creative Directions**
   Propose 3-5 distinct creative directions:
   - Direction name
   - Mood and visual style
   - Color palette suggestion
   - Typography style
   - Reference examples
   - Rationale

4. **Usage Guidelines**
   - Primary use cases
   - Minimum sizes
   - Clear space requirements
   - Background requirements
   - Forbidden treatments

5. **Evaluation Criteria**
   - Memorability
   - Scalability
   - Versatility
   - Relevance to brand
   - Uniqueness

### Output

1. Create logo brief at:
   `workspaces/{mvp-slug}/graphics/logo/logo-brief.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.branding` to "in-progress"
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ Logo brief complete for {mvp-name}
   �� Output: workspaces/{mvp-slug}/graphics/logo/logo-brief.md
   
   Next step: /gd/brand-assets {mvp-slug}
   ```
