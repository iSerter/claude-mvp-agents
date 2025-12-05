# claude-mvp-agents

A set of Claude Code agents, skills, and slash commands for building web-app MVPs quickly—from product discovery to implementation-ready specs. **Now supports multiple concurrent MVPs with isolated workspaces.**

## Overview

This repository provides a complete toolkit for taking a product idea from concept to implementation-ready specs using Claude Code's agent system. It includes:

- **7 Specialized Agents** for product, design, branding, and development work
- **15+ Skills** providing deep-dive methodologies
- **25+ Slash Commands** for quick artifact generation
- **Multi-MVP Support** with isolated workspaces

All artifacts are output to `workspaces/{mvp-slug}/*` for organized, per-project workflows.

---

## Quick Start

### 1. Start a New MVP

```
/start-mvp my-awesome-app "An AI-powered task management app for remote teams"
```

This creates:
```
workspaces/my-awesome-app/
├── config.json           # MVP configuration and metadata
├── README.md             # Workspace overview
├── product/              # Product Owner artifacts
├── ui-ux/                # UI/UX Designer artifacts
├── graphics/             # Graphic Designer artifacts
└── code/                 # Developer artifacts
```

### 2. Run the Discovery-to-Specs Pipeline

```
# Product Discovery
/po/discover my-awesome-app
/po/research my-awesome-app
/po/strategy my-awesome-app
/po/prioritize my-awesome-app
/po/prd my-awesome-app

# UI/UX Design
/ui/design-brief my-awesome-app
/ui/information-architecture my-awesome-app
/ui/wireframes my-awesome-app
/ui/high-fidelity-plan my-awesome-app

# Branding
/gd/logo-briefs my-awesome-app
/gd/brand-assets my-awesome-app

# Development Planning
/dev/architecture my-awesome-app
/dev/backend my-awesome-app
/dev/frontend my-awesome-app
/dev/mobile my-awesome-app react-native
```

---

## Agents

### Product Owner (`product-owner`)
End-to-end product management for new product discovery and definition.

**Skills:** `po-discovery`, `po-market-research`, `po-vision-strategy`, `po-prioritization`, `po-prd-decomposition`

**Use for:** Defining problems, market research, product strategy, PRDs, epics, and user stories.

### UI/UX Designer (`ui-ux-designer`)
Senior product designer for translating PRDs into design specifications.

**Skills:** `ux-design-brief`, `ux-information-architecture`, `ux-wireframes`, `ux-design-system-integration`, `ux-ui-plan-review`, `ux-ui-plan-revision`, `ux-high-fidelity-plan`

**Use for:** Design briefs, information architecture, user flows, wireframes, and high-fidelity UI plans.

### Graphic Designer (`graphic-designer`)
Brand and marketing design for visual identity work.

**Skills:** `gd-logo-briefs`, `gd-brand-assets`, `gd-ad-banner-briefs`

**Use for:** Logo design briefs, brand asset specifications, and ad banner planning.

### Nest.js Developer (`nestjs-developer`)
Senior backend developer specialized in Nest.js and Node.js/TypeScript.

**Use for:** API design, backend architecture, database design, authentication, and server-side implementation planning.

### Next.js Developer (`nextjs-developer`)
Senior frontend developer specialized in Next.js and React/TypeScript.

**Use for:** Frontend architecture, component design, state management, and client-side implementation planning.

### React Native Developer (`react-native-developer`)
Senior mobile developer specialized in React Native and Expo.

**Use for:** Cross-platform mobile architecture, native integrations, and React Native implementation planning.

### Flutter Developer (`flutter-developer`)
Senior mobile developer specialized in Flutter and Dart.

**Use for:** Cross-platform mobile architecture, widget design, and Flutter implementation planning.

---

## Slash Commands

### Workspace Commands

| Command | Description |
|---------|-------------|
| `/start-mvp <name> "<brief>"` | Initialize a new MVP workspace |

### Product Owner Commands (`po/`)

| Command | Description | Output |
|---------|-------------|--------|
| `/po/discover <mvp-slug>` | User discovery—problems, personas, JTBD | `workspaces/{slug}/product/discovery/` |
| `/po/research <mvp-slug>` | Market & competitor analysis | `workspaces/{slug}/product/market-research/` |
| `/po/strategy <mvp-slug>` | Vision, value props, strategic pillars | `workspaces/{slug}/product/strategy/` |
| `/po/prioritize <mvp-slug>` | RICE scoring, MoSCoW, roadmap | `workspaces/{slug}/product/roadmap/` |
| `/po/prd <mvp-slug>` | Full PRD with epics, stories, acceptance criteria | `workspaces/{slug}/product/prd/` |

### UI/UX Designer Commands (`ui/`)

| Command | Description | Output |
|---------|-------------|--------|
| `/ui/design-brief <mvp-slug>` | Design brief from PRD | `workspaces/{slug}/ui-ux/design-brief/` |
| `/ui/information-architecture <mvp-slug>` | IA and user flows | `workspaces/{slug}/ui-ux/information-architecture/` |
| `/ui/wireframes <mvp-slug>` | Wireframe specifications | `workspaces/{slug}/ui-ux/wireframes/` |
| `/ui/design-system-integration <mvp-slug>` | Design system mapping | `workspaces/{slug}/ui-ux/design-system/` |
| `/ui/plan-review <mvp-slug>` | Review UI plan | `workspaces/{slug}/ui-ux/reviews/` |
| `/ui/plan-revision <mvp-slug>` | Revise based on feedback | `workspaces/{slug}/ui-ux/reviews/` |
| `/ui/high-fidelity-plan <mvp-slug>` | Hi-fi design specs | `workspaces/{slug}/ui-ux/high-fidelity/` |

### Graphic Designer Commands (`gd/`)

| Command | Description | Output |
|---------|-------------|--------|
| `/gd/logo-briefs <mvp-slug>` | Logo design brief with creative directions | `workspaces/{slug}/graphics/logo/` |
| `/gd/brand-assets <mvp-slug>` | Brand asset specifications | `workspaces/{slug}/graphics/brand-assets/` |
| `/gd/ad-banner-briefs <mvp-slug>` | Ad banner design briefs | `workspaces/{slug}/graphics/ad-banners/` |

### Developer Commands (`dev/`)

| Command | Description | Output |
|---------|-------------|--------|
| `/dev/architecture <mvp-slug>` | Technical architecture document | `workspaces/{slug}/code/architecture/` |
| `/dev/backend <mvp-slug>` | Backend implementation plan (Nest.js) | `workspaces/{slug}/code/backend/` |
| `/dev/frontend <mvp-slug>` | Frontend implementation plan (Next.js) | `workspaces/{slug}/code/frontend/` |
| `/dev/mobile <mvp-slug> [framework]` | Mobile implementation plan | `workspaces/{slug}/code/mobile/` |
| `/dev/api-spec <mvp-slug>` | OpenAPI specification | `workspaces/{slug}/code/backend/` |
| `/dev/database <mvp-slug>` | Database schema design | `workspaces/{slug}/code/backend/database/` |

---

## Workspace Structure

Each MVP gets its own isolated workspace:

```
workspaces/{mvp-slug}/
├── config.json                 # MVP metadata and phase tracking
├── README.md                   # Workspace overview
│
├── product/                    # Product Owner artifacts
│   ├── discovery/
│   ├── market-research/
│   ├── strategy/
│   ├── roadmap/
│   └── prd/
│       └── epics/
│
├── ui-ux/                      # UI/UX Designer artifacts
│   ├── design-brief/
│   ├── information-architecture/
│   ├── wireframes/
│   │   └── screens/
│   ├── design-system/
│   ├── high-fidelity/
│   └── reviews/
│
├── graphics/                   # Graphic Designer artifacts
│   ├── logo/
│   ├── brand-assets/
│   └── ad-banners/
│
└── code/                       # Developer artifacts
    ├── architecture/
    ├── backend/
    │   └── database/
    ├── frontend/
    └── mobile/
```

### config.json Structure

```json
{
  "name": "My Awesome App",
  "slug": "my-awesome-app",
  "brief": "An AI-powered task management app for remote teams",
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-15T14:45:00Z",
  "status": "in-progress",
  "phases": {
    "discovery": "completed",
    "research": "completed",
    "strategy": "completed",
    "prd": "completed",
    "design": "in-progress",
    "branding": "pending",
    "development": "pending"
  },
  "artifacts": [
    "product/discovery/discovery.md",
    "product/market-research/market-research.md"
  ]
}
```

---

## Recommended Workflow

### Phase 1: Product Definition
```
/start-mvp my-app "Description of the MVP"
/po/discover my-app
/po/research my-app
/po/strategy my-app
/po/prioritize my-app
/po/prd my-app
```

### Phase 2: UI/UX Design
```
/ui/design-brief my-app
/ui/information-architecture my-app
/ui/wireframes my-app
/ui/design-system-integration my-app
/ui/high-fidelity-plan my-app
```

### Phase 3: Branding
```
/gd/logo-briefs my-app
/gd/brand-assets my-app
```

### Phase 4: Development Planning
```
/dev/architecture my-app
/dev/database my-app
/dev/api-spec my-app
/dev/backend my-app
/dev/frontend my-app
/dev/mobile my-app react-native
```

---

## Multiple MVPs

You can work on multiple MVPs simultaneously:

```
/start-mvp project-alpha "First project idea"
/start-mvp project-beta "Second project idea"

# Work on project-alpha
/po/discover project-alpha

# Switch to project-beta
/po/discover project-beta

# Each has its own isolated workspace
```

---

## Installation

1. Clone this repository into your project or as a standalone workspace
2. Ensure Claude Code is installed and configured
3. The agents, skills, and commands are automatically available

```bash
# Clone into existing project
git clone https://github.com/iSerter/claude-mvp-agents.git .claude-agents
cp -r .claude-agents/.claude .

# Or use as standalone workspace
git clone https://github.com/iSerter/claude-mvp-agents.git
cd claude-mvp-agents
```

---

## Contributing

Contributions welcome! Areas of interest:
- New agents (QA, DevOps, etc.)
- Additional skills for existing agents
- Workflow automation improvements
- Documentation and examples

---

## License

MIT License - see [LICENSE](LICENSE) for details.
