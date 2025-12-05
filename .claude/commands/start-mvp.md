---
description: Initialize a new MVP workspace with config and folder structure
argument-hint: <mvp-name> "<brief description of the MVP idea>"
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Start New MVP

## Context

- MVP Input: $ARGUMENTS
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are initializing a new MVP workspace. This creates an isolated workspace where all artifacts for this MVP will be stored.

### Process

1. **Parse Input**
   - Extract the MVP name (first argument, will be slugified)
   - Extract the brief description (remaining text or quoted string)
   - If only one argument is provided, use it as both name and brief

2. **Generate MVP Slug**
   - Convert MVP name to URL-friendly slug (lowercase, hyphens, no special chars)
   - Example: "AI Docs Assistant" → "ai-docs-assistant"

3. **Create Workspace Structure**
   
   Create the following directory structure:
   ```
   workspaces/{mvp-slug}/
   ├── config.json           # MVP configuration and metadata
   ├── product/              # Product Owner artifacts
   │   ├── discovery/
   │   ├── market-research/
   │   ├── strategy/
   │   ├── roadmap/
   │   └── prd/
   ├── ui-ux/                # UI/UX Designer artifacts
   │   ├── design-brief/
   │   ├── information-architecture/
   │   ├── wireframes/
   │   ├── design-system/
   │   └── high-fidelity/
   ├── graphics/             # Graphic Designer artifacts
   │   ├── logo/
   │   ├── brand-assets/
   │   └── ad-banners/
   └── code/                 # Developer artifacts
       ├── architecture/
       ├── backend/
       ├── frontend/
       └── mobile/
   ```

4. **Generate config.json**
   
   Create `workspaces/{mvp-slug}/config.json` with:
   ```json
   {
     "name": "Human-readable MVP name",
     "slug": "mvp-slug",
     "brief": "The brief description provided",
     "createdAt": "ISO timestamp",
     "updatedAt": "ISO timestamp",
     "status": "initialized",
     "phases": {
       "discovery": "pending",
       "research": "pending",
       "strategy": "pending",
       "prd": "pending",
       "design": "pending",
       "branding": "pending",
       "development": "pending"
     },
     "artifacts": []
   }
   ```

5. **Create Workspace README**
   
   Create `workspaces/{mvp-slug}/README.md` with:
   - MVP name and brief
   - Workspace structure overview
   - Quick links to run next commands
   - Status tracking section

### Output

After creating the workspace:

1. Display a summary:
   - Workspace location
   - Config file location
   - Folder structure created

2. Suggest next steps:
   ```
   Your MVP workspace is ready! Next steps:
   
   1. Run discovery: /mvp-discover {mvp-slug}
   2. Run research:  /mvp-research {mvp-slug}
   3. Run strategy:  /mvp-strategy {mvp-slug}
   4. Generate PRD:  /mvp-prd {mvp-slug}
   ```

### Example

Input: `start-mvp ai-docs-assistant "An AI-powered documentation assistant that helps developers write better docs"`

Creates:
- `workspaces/ai-docs-assistant/config.json`
- `workspaces/ai-docs-assistant/README.md`
- Full folder structure as above
