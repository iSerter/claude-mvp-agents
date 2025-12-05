---
name: dev-frontend-implementation
description: Plan frontend implementation with component architecture, state management, and UI patterns. Use when creating detailed frontend development plans for Next.js, React, or similar frameworks.
---

# Frontend Implementation Planning

## Purpose

Use this Skill to create detailed frontend implementation plans:
- Component architecture and hierarchy
- State management strategy
- Routing and navigation
- API integration patterns
- Implementation task breakdown

## Inputs

The main agent should provide:
- UI/UX design specifications
- Wireframes or high-fidelity designs
- API specifications
- Technical architecture document

## Process

### 1. Project Structure

Design the folder structure:

```
src/
├── app/                      # Next.js App Router
│   ├── (auth)/              # Auth route group
│   │   ├── login/
│   │   └── register/
│   ├── (dashboard)/         # Protected routes
│   │   ├── layout.tsx
│   │   ├── projects/
│   │   └── settings/
│   ├── api/                 # API routes (if needed)
│   ├── layout.tsx           # Root layout
│   └── page.tsx             # Home page
│
├── components/
│   ├── ui/                  # Base UI components (shadcn)
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   └── ...
│   ├── features/            # Feature components
│   │   ├── projects/
│   │   └── tasks/
│   └── layouts/             # Layout components
│       ├── header.tsx
│       ├── sidebar.tsx
│       └── footer.tsx
│
├── lib/                     # Utilities
│   ├── api/                 # API client
│   ├── utils/               # Helper functions
│   └── validations/         # Zod schemas
│
├── hooks/                   # Custom hooks
│   ├── use-auth.ts
│   ├── use-projects.ts
│   └── use-debounce.ts
│
├── stores/                  # Global state
│   ├── auth-store.ts
│   └── ui-store.ts
│
├── types/                   # TypeScript types
│   ├── api.ts
│   └── models.ts
│
└── styles/                  # Global styles
    └── globals.css
```

### 2. Component Architecture

Define component hierarchy:

```
Page: ProjectsPage
├── Layout: DashboardLayout
│   ├── Header
│   │   ├── Logo
│   │   ├── Navigation
│   │   └── UserMenu
│   ├── Sidebar
│   │   └── SidebarNav
│   └── Main
│       └── ProjectsPage
│           ├── ProjectsHeader
│           │   ├── PageTitle
│           │   ├── SearchInput
│           │   └── CreateProjectButton
│           ├── ProjectFilters
│           │   ├── StatusFilter
│           │   └── SortSelect
│           └── ProjectList
│               ├── ProjectCard (repeated)
│               │   ├── ProjectTitle
│               │   ├── ProjectMeta
│               │   └── ProjectActions
│               └── EmptyState (conditional)
```

### 3. Component Specifications

For each key component:

```typescript
// ProjectCard component spec
interface ProjectCardProps {
  project: Project;
  onEdit?: (id: string) => void;
  onDelete?: (id: string) => void;
  variant?: 'default' | 'compact';
}

// States to handle:
// - Default (populated)
// - Loading skeleton
// - Hover state
// - Selected state

// Responsive behavior:
// - Desktop: Horizontal card layout
// - Tablet: Compact horizontal
// - Mobile: Vertical stack

// Accessibility:
// - Focusable container
// - Keyboard navigation for actions
// - Screen reader labels
```

### 4. State Management Strategy

Plan state organization:

```typescript
// Server state (TanStack Query)
// - Projects list and details
// - User data
// - Any data from API

// Client state (Zustand)
// - UI state (sidebar open, theme)
// - Auth state (current user, tokens)
// - Form state (temporary)

// URL state
// - Filters, sorting, pagination
// - Current tab/view
// - Modal/drawer state

// Example Zustand store
interface UIStore {
  sidebarOpen: boolean;
  theme: 'light' | 'dark' | 'system';
  toggleSidebar: () => void;
  setTheme: (theme: Theme) => void;
}

// Example TanStack Query hook
const useProjects = (filters: ProjectFilters) => {
  return useQuery({
    queryKey: ['projects', filters],
    queryFn: () => projectsApi.list(filters),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });
};
```

### 5. API Integration

Design API client:

```typescript
// lib/api/client.ts
const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  timeout: 10000,
});

// Request interceptor for auth
apiClient.interceptors.request.use((config) => {
  const token = getAccessToken();
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response interceptor for errors
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      await refreshToken();
      return apiClient.request(error.config);
    }
    return Promise.reject(error);
  }
);

// lib/api/projects.ts
export const projectsApi = {
  list: (params: ProjectListParams) => 
    apiClient.get<PaginatedResponse<Project>>('/projects', { params }),
  
  get: (id: string) => 
    apiClient.get<Project>(`/projects/${id}`),
  
  create: (data: CreateProjectInput) => 
    apiClient.post<Project>('/projects', data),
  
  update: (id: string, data: UpdateProjectInput) => 
    apiClient.patch<Project>(`/projects/${id}`, data),
  
  delete: (id: string) => 
    apiClient.delete(`/projects/${id}`),
};
```

### 6. Form Handling

Plan form implementation:

```typescript
// Using React Hook Form + Zod
const projectSchema = z.object({
  name: z.string().min(3).max(100),
  description: z.string().max(1000).optional(),
  visibility: z.enum(['public', 'private']),
});

type ProjectFormData = z.infer<typeof projectSchema>;

function CreateProjectForm() {
  const form = useForm<ProjectFormData>({
    resolver: zodResolver(projectSchema),
    defaultValues: {
      name: '',
      description: '',
      visibility: 'private',
    },
  });

  const createMutation = useMutation({
    mutationFn: projectsApi.create,
    onSuccess: () => {
      queryClient.invalidateQueries(['projects']);
      toast.success('Project created');
    },
    onError: (error) => {
      toast.error(getErrorMessage(error));
    },
  });

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(createMutation.mutate)}>
        {/* Form fields */}
      </form>
    </Form>
  );
}
```

### 7. Routing Structure

Define routes:

```typescript
// Route structure
const routes = {
  // Public routes
  home: '/',
  login: '/login',
  register: '/register',
  forgotPassword: '/forgot-password',
  
  // Protected routes
  dashboard: '/dashboard',
  projects: '/projects',
  project: (id: string) => `/projects/${id}`,
  projectSettings: (id: string) => `/projects/${id}/settings`,
  settings: '/settings',
  profile: '/settings/profile',
};

// Route protection (middleware.ts)
export function middleware(request: NextRequest) {
  const token = request.cookies.get('token');
  const isProtectedRoute = request.nextUrl.pathname.startsWith('/dashboard') ||
                          request.nextUrl.pathname.startsWith('/projects');
  
  if (isProtectedRoute && !token) {
    return NextResponse.redirect(new URL('/login', request.url));
  }
}
```

### 8. Loading & Error States

Plan UI states:

```typescript
// Loading states
// - Page skeleton
// - Component skeleton
// - Button loading spinner
// - Progress indicators

// Error states
// - 404 page
// - Error boundary
// - Inline error messages
// - Toast notifications

// Empty states
// - No projects
// - No search results
// - No permissions

// Example error boundary
class ErrorBoundary extends Component {
  state = { hasError: false, error: null };

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback error={this.state.error} />;
    }
    return this.props.children;
  }
}
```

### 9. Responsive Design

Plan breakpoints:

```typescript
// Tailwind breakpoints
// sm: 640px
// md: 768px
// lg: 1024px
// xl: 1280px
// 2xl: 1536px

// Component responsive behavior
// Mobile: Stack layout, bottom navigation
// Tablet: Side navigation, hybrid layouts
// Desktop: Full sidebar, expanded views

// Example responsive component
<div className="
  grid gap-4
  grid-cols-1        // Mobile: single column
  sm:grid-cols-2     // Tablet: 2 columns
  lg:grid-cols-3     // Desktop: 3 columns
  xl:grid-cols-4     // Large: 4 columns
">
  {projects.map(project => (
    <ProjectCard key={project.id} project={project} />
  ))}
</div>
```

### 10. Implementation Tasks

Break down into tasks:

| Task | Priority | Estimate | Dependencies |
|------|----------|----------|--------------|
| Setup Next.js project with TypeScript | High | 2h | - |
| Configure Tailwind + shadcn/ui | High | 2h | Setup |
| Setup API client + TanStack Query | High | 3h | Setup |
| Implement auth flow | High | 8h | API client |
| Build layout components | High | 4h | Tailwind |
| Build project list page | High | 6h | Layout, API |
| Build project detail page | High | 6h | Layout, API |
| Build create/edit forms | Medium | 6h | Project pages |
| Add loading states | Medium | 3h | All pages |
| Add error handling | Medium | 3h | All pages |
| Implement responsive design | Medium | 4h | All components |
| Write component tests | Low | 6h | All components |

## Output Format

Produce documentation with:
- Folder structure diagram
- Component hierarchy
- Component specifications
- State management plan
- API integration patterns
- Route structure
- Responsive breakpoints
- Task breakdown with estimates

## Best Practices

- **Component composition**: Small, reusable components
- **Colocation**: Keep related code together
- **Type safety**: TypeScript for all components
- **Server components**: Use by default, client only when needed
- **Optimistic updates**: Better UX for mutations
- **Accessibility**: ARIA labels, keyboard navigation
