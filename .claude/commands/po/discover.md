---
description: Run user discovery to understand problems, personas, and Jobs-To-Be-Done for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Product Discovery

## Context

- MVP Slug: $ARGUMENTS
- Config Path: `workspaces/$ARGUMENTS/config.json`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are performing **Product Discovery** for the MVP workspace specified above.

### Pre-check

1. **Verify MVP exists**: Read `workspaces/$ARGUMENTS/config.json` using the Read tool
2. If the file doesn't exist, inform the user:
   ```
   ❌ MVP workspace not found: $ARGUMENTS
   
   Run /start-mvp {mvp-name} "description" first to create a workspace.
   ```
3. Load the MVP brief from config.json to understand the product idea

### Process

Use the **po-discovery** skill to deeply understand users and their problems.

1. **Clarify the product idea**
   - Restate the product idea from the MVP config's `brief` field
   - Infer the target audience
   - Identify the likely business objective (acquisition, retention, revenue, etc.)
   - Note any assumptions you're making

2. **User Segments & Personas**
   - Identify 2-4 primary user segments
   - For each segment, describe:
     - Role/profile
     - Goals and motivations
     - Primary pains and friction points
     - Triggers that make the problem urgent

3. **Jobs-To-Be-Done (JTBD)**
   - For each segment, write 3-5 core jobs in JTBD format:
     - "When [situation], I want to [motivation], so I can [expected outcome]."

4. **Problem Statements**
   - Synthesize 5-10 problem statements:
     - "Users in [segment] struggle with [problem], which leads to [impact]."
   - Rank by apparent severity/frequency

5. **Confidence & Validation**
   - Assign confidence levels (low/medium/high) to key assumptions
   - Suggest validation experiments (interviews, surveys, data analysis)

### Output

1. Create discovery document at:
   `workspaces/{mvp-slug}/product/discovery/discovery.md`

2. Update `workspaces/{mvp-slug}/config.json`:
   - Set `phases.discovery` to "completed"
   - Add artifact path to `artifacts` array
   - Update `updatedAt` timestamp

3. Display summary:
   ```
   ✅ Discovery complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/product/discovery/discovery.md
   
   Next step: /po/research {mvp-slug}
   ```
