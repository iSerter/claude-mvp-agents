---
description: Create ad banner design briefs for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Ad Banner Design Briefs

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Brand Guidelines: !`cat workspaces/$ARGUMENTS/graphics/brand-assets/brand-guidelines.md 2>/dev/null || echo ""`
- Strategy: !`cat workspaces/$ARGUMENTS/product/strategy/strategy.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `graphic-designer` agent creating **Ad Banner Design Briefs** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load brand guidelines and strategy for context

### Process

Use the **gd-ad-banner-briefs** skill.

1. **Campaign Overview**
   - Campaign objectives
   - Target audience
   - Key messages
   - Call-to-action options

2. **Banner Sizes Required**
   - Google Display Network sizes
   - Social media sizes (Facebook, Instagram, LinkedIn, Twitter)
   - Custom sizes if needed
   - Priority order

3. **Creative Concepts**
   Propose 2-3 creative concepts:
   - Concept name
   - Visual approach
   - Copy direction
   - Animation considerations (if applicable)

4. **Per-Size Specifications**
   For each banner size:
   - Dimensions
   - Safe zones
   - Text limits
   - File format and size requirements

5. **Copy Guidelines**
   - Headlines (character limits per size)
   - Body copy options
   - CTA button text options
   - Legal/disclaimer requirements

6. **Production Checklist**
   - All required sizes
   - Format requirements (static, animated, HTML5)
   - Naming conventions
   - Delivery specifications

### Output

1. Create ad banner briefs at:
   `workspaces/{mvp-slug}/graphics/ad-banners/ad-banner-briefs.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ Ad banner briefs complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/graphics/ad-banners/ad-banner-briefs.md
   
   Graphics phase complete!
   ```
