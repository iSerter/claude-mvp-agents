---
description: Create frontend implementation plan using Next.js for an MVP
argument-hint: <mvp-slug>
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Frontend Implementation Plan (Next.js)

## Context

- MVP Slug: $ARGUMENTS
- Config Path: `workspaces/$ARGUMENTS/config.json`
- Architecture Path: `workspaces/$ARGUMENTS/code/architecture/architecture.md`
- Hi-Fi Specs Path: `workspaces/$ARGUMENTS/ui-ux/high-fidelity/high-fidelity-specs.md`
- Design Tokens Path: `workspaces/$ARGUMENTS/ui-ux/high-fidelity/design-tokens.json`
- Wireframes Path: `workspaces/$ARGUMENTS/ui-ux/wireframes/wireframes.md`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are the `nextjs-developer` agent creating a **Frontend Implementation Plan** for the MVP specified above.

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Load architecture and design specs for context

### Process

1. **Project Structure**
   ```
   src/
   ├── app/               # Next.js App Router
   ├── components/        # React components
   │   ├── ui/           # shadcn/ui components
   │   └── features/     # Feature components
   ├── lib/              # Utilities
   ├── hooks/            # Custom hooks
   ├── stores/           # State management
   └── types/            # TypeScript types
   ```

2. **Route Structure**
   - App Router pages and layouts
   - Route groups
   - Dynamic routes
   - API routes (if needed)

3. **Component Architecture**
   For each major component:
   - Component name and purpose
   - Props interface
   - State requirements
   - Child components
   - Responsive behavior

4. **State Management**
   - Global state (Zustand/Jotai)
   - Server state (TanStack Query)
   - Form state (React Hook Form)
   - URL state

5. **API Integration**
   - API client setup
   - Request/response typing
   - Error handling
   - Loading states

6. **Styling System**
   - Tailwind configuration
   - Design tokens integration
   - Component variants (CVA)
   - Dark mode support

7. **Testing Strategy**
   - Component testing approach
   - Integration tests
   - E2E tests with Playwright

8. **Implementation Tasks**
   - Task breakdown with estimates
   - Component development order
   - Dependencies between tasks

### Output

1. Create frontend plan at:
   `workspaces/{mvp-slug}/code/frontend/frontend-plan.md`

2. Create component spec at:
   `workspaces/{mvp-slug}/code/frontend/component-spec.md`

3. Create route structure at:
   `workspaces/{mvp-slug}/code/frontend/routes.md`

4. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

5. Display summary:
   ```
   ✅ Frontend plan complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/code/frontend/
   
   Tech stack: Next.js 14 + TypeScript + Tailwind + shadcn/ui
   
   Next: /dev/mobile {mvp-slug} (if mobile needed)
   ```
