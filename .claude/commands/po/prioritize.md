---
description: Score and prioritize features using RICE framework and create a roadmap for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Feature Prioritization & Roadmap

## Context

- MVP Slug: $ARGUMENTS
- Config: !`cat workspaces/$ARGUMENTS/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Strategy: !`cat workspaces/$ARGUMENTS/product/strategy/strategy.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are performing **Feature Prioritization** for the MVP workspace specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load strategy document for context

### Process

Use the **po-prioritization** skill to create a prioritized backlog and roadmap.

1. **Feature Inventory**
   - List all potential features or opportunities (aim for 10-20)
   - Group into themes where helpful
   - Write each as a clear, action-oriented statement

2. **RICE Scoring**
   - For each feature, estimate:
     - **Reach:** Users affected per quarter (number or T-shirt size)
     - **Impact:** Effect per user (0.25=minimal, 0.5=low, 1=medium, 2=high, 3=massive)
     - **Confidence:** How sure are we? (0.5=low, 0.8=medium, 1.0=high)
     - **Effort:** Person-weeks or T-shirt size (S=1, M=2, L=4, XL=8)
   - Calculate RICE = (Reach × Impact × Confidence) / Effort

3. **Prioritization Table**
   - Create a markdown table with columns:
     - Feature | Theme | Reach | Impact | Confidence | Effort | RICE Score | Priority

4. **MoSCoW Classification**
   - Classify features as:
     - **Must Have:** Critical for MVP launch
     - **Should Have:** Important but not blocking
     - **Could Have:** Nice to have
     - **Won't Have (now):** Explicitly out of scope

5. **Phased Roadmap**
   - **Now (MVP):** High RICE, low risk, foundational
   - **Next (v1.1-v1.2):** Medium RICE, builds on MVP
   - **Later (v2+):** Lower priority, experimental, or complex
   - Note dependencies between features

6. **Assumptions & Risks**
   - List key assumptions behind the scoring
   - Highlight high-risk bets and mitigation strategies

### Output

1. Create roadmap document at:
   `workspaces/{mvp-slug}/product/roadmap/roadmap.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ Prioritization complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/product/roadmap/roadmap.md
   
   Next step: /po/prd {mvp-slug}
   ```
