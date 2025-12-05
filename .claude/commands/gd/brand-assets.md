---
description: Create brand asset specifications and guidelines for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Brand Assets Plan

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Logo Brief: !`cat workspaces/$ARGUMENTS/graphics/logo/logo-brief.md 2>/dev/null || echo ""`
- Strategy: !`cat workspaces/$ARGUMENTS/product/strategy/strategy.md 2>/dev/null || echo ""`
- Design Tokens: !`cat workspaces/$ARGUMENTS/ui-ux/high-fidelity/design-tokens.json 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `graphic-designer` agent creating a **Brand Assets Plan** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load logo brief and strategy for context

### Process

Use the **gd-brand-assets** skill.

1. **Brand Guidelines**
   - Brand story and values
   - Brand voice and tone
   - Visual identity principles
   - Do's and Don'ts

2. **Color System**
   - Primary colors (with hex, RGB, CMYK, Pantone)
   - Secondary colors
   - Accent colors
   - Semantic colors (success, error, warning, info)
   - Color usage guidelines and ratios

3. **Typography System**
   - Primary typeface (with fallbacks)
   - Secondary typeface
   - Type scale and hierarchy
   - Usage guidelines per context

4. **Imagery Style**
   - Photography style guidelines
   - Illustration style guidelines
   - Iconography style and sources
   - Image treatment and filters

5. **Brand Applications**
   - Social media profiles and templates
   - Email signatures
   - Presentation templates
   - Business cards
   - Marketing collateral specs

6. **Asset Inventory**
   - List all required assets
   - Priority and timeline
   - Specifications per asset

### Output

1. Create brand guidelines at:
   `workspaces/{mvp-slug}/graphics/brand-assets/brand-guidelines.md`

2. Create color palette at:
   `workspaces/{mvp-slug}/graphics/brand-assets/color-palette.json`

3. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.branding` to "completed"
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

4. Display summary:
   ```
   ✅ Brand assets complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/graphics/brand-assets/
   
   Branding phase complete! Next:
   - Ad Banners: /gd/ad-banner-briefs {mvp-slug}
   - Development: /dev/architecture {mvp-slug}
   ```
