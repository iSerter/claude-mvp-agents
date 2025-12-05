---
name: dev-testing-strategy
description: Design comprehensive testing strategies including unit, integration, and E2E tests. Use when planning test architecture, coverage goals, or CI/CD test pipelines.
---

# Testing Strategy Design

## Purpose

Use this Skill to create comprehensive testing plans:
- Test architecture and organization
- Unit, integration, and E2E test strategies
- Coverage goals and metrics
- CI/CD integration

## Inputs

The main agent should provide:
- Application architecture
- Critical user flows
- Performance requirements
- Team testing experience level

## Process

### 1. Testing Pyramid

Design test distribution:

```
          /\
         /  \
        / E2E \        5-10%: Critical user journeys
       /  Tests \
      /──────────\
     / Integration \   20-30%: API contracts, component integration
    /    Tests      \
   /──────────────────\
  /    Unit Tests      \  60-70%: Business logic, utilities
 /______________________\
```

### 2. Unit Testing Strategy

Plan unit test coverage:

```typescript
// What to unit test:
// - Pure functions and utilities
// - Business logic in services
// - State reducers
// - Data transformations
// - Validation logic

// Testing pattern: Arrange-Act-Assert
describe('ProjectService', () => {
  describe('calculateProgress', () => {
    it('should return 0 when no tasks', () => {
      // Arrange
      const project = createProject({ tasks: [] });
      
      // Act
      const progress = calculateProgress(project);
      
      // Assert
      expect(progress).toBe(0);
    });

    it('should calculate percentage of completed tasks', () => {
      // Arrange
      const project = createProject({
        tasks: [
          createTask({ status: 'done' }),
          createTask({ status: 'done' }),
          createTask({ status: 'in-progress' }),
          createTask({ status: 'todo' }),
        ],
      });
      
      // Act
      const progress = calculateProgress(project);
      
      // Assert
      expect(progress).toBe(50);
    });
  });
});

// Mock patterns
jest.mock('@/lib/api');

const mockProjectsApi = projectsApi as jest.Mocked<typeof projectsApi>;
mockProjectsApi.list.mockResolvedValue({ data: [], meta: { total: 0 } });
```

### 3. Integration Testing

Plan integration tests:

```typescript
// What to integration test:
// - API endpoints (request/response)
// - Database operations
// - External service interactions
// - Component + hooks combinations

// Backend integration test (Nest.js)
describe('ProjectsController (e2e)', () => {
  let app: INestApplication;
  let prisma: PrismaService;

  beforeAll(async () => {
    const moduleFixture = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    prisma = app.get(PrismaService);
    await app.init();
  });

  afterAll(async () => {
    await app.close();
  });

  beforeEach(async () => {
    await prisma.cleanDatabase();
  });

  describe('GET /projects', () => {
    it('should return paginated projects', async () => {
      // Seed data
      await prisma.project.createMany({
        data: [
          { name: 'Project 1', ownerId: testUserId },
          { name: 'Project 2', ownerId: testUserId },
        ],
      });

      // Make request
      const response = await request(app.getHttpServer())
        .get('/projects')
        .set('Authorization', `Bearer ${testToken}`)
        .expect(200);

      // Assert
      expect(response.body.data).toHaveLength(2);
      expect(response.body.meta.total).toBe(2);
    });
  });
});

// Frontend integration test (React)
describe('ProjectList', () => {
  it('should fetch and display projects', async () => {
    // Setup MSW handler
    server.use(
      rest.get('/api/projects', (req, res, ctx) => {
        return res(ctx.json({
          data: [mockProject],
          meta: { total: 1 },
        }));
      })
    );

    // Render with providers
    render(
      <QueryClientProvider client={queryClient}>
        <ProjectList />
      </QueryClientProvider>
    );

    // Wait for data
    await waitFor(() => {
      expect(screen.getByText(mockProject.name)).toBeInTheDocument();
    });
  });
});
```

### 4. E2E Testing

Plan end-to-end tests:

```typescript
// What to E2E test:
// - Critical user journeys
// - Auth flows
// - Payment flows
// - Core business flows

// Web E2E with Playwright
import { test, expect } from '@playwright/test';

test.describe('Project Management Flow', () => {
  test.beforeEach(async ({ page }) => {
    // Login
    await page.goto('/login');
    await page.fill('[data-testid="email"]', 'test@example.com');
    await page.fill('[data-testid="password"]', 'password');
    await page.click('[data-testid="login-button"]');
    await page.waitForURL('/dashboard');
  });

  test('should create a new project', async ({ page }) => {
    // Navigate to projects
    await page.click('[data-testid="nav-projects"]');
    
    // Open create dialog
    await page.click('[data-testid="create-project-button"]');
    
    // Fill form
    await page.fill('[data-testid="project-name"]', 'New Project');
    await page.selectOption('[data-testid="visibility"]', 'private');
    
    // Submit
    await page.click('[data-testid="submit-button"]');
    
    // Verify
    await expect(page.locator('[data-testid="project-card"]')).toContainText('New Project');
  });

  test('should add tasks to project', async ({ page }) => {
    // Navigate to project
    await page.click('[data-testid="project-card"]');
    
    // Add task
    await page.click('[data-testid="add-task-button"]');
    await page.fill('[data-testid="task-title"]', 'New Task');
    await page.click('[data-testid="save-task"]');
    
    // Verify
    await expect(page.locator('[data-testid="task-item"]')).toContainText('New Task');
  });
});

// Mobile E2E with Detox
describe('Project Management', () => {
  beforeAll(async () => {
    await device.launchApp({ newInstance: true });
  });

  beforeEach(async () => {
    await device.reloadReactNative();
  });

  it('should create project', async () => {
    await element(by.id('create-fab')).tap();
    await element(by.id('project-name-input')).typeText('New Project');
    await element(by.id('create-button')).tap();
    await expect(element(by.text('New Project'))).toBeVisible();
  });
});
```

### 5. Test Data Management

Plan test fixtures:

```typescript
// factories/project.factory.ts
import { faker } from '@faker-js/faker';

export function createProject(overrides: Partial<Project> = {}): Project {
  return {
    id: faker.string.uuid(),
    name: faker.company.name(),
    description: faker.lorem.paragraph(),
    visibility: 'private',
    ownerId: faker.string.uuid(),
    createdAt: faker.date.past().toISOString(),
    updatedAt: faker.date.recent().toISOString(),
    ...overrides,
  };
}

export function createProjectList(count: number): Project[] {
  return Array.from({ length: count }, () => createProject());
}

// Seeding for E2E
// seeds/test-data.ts
export async function seedTestData(prisma: PrismaClient) {
  const user = await prisma.user.create({
    data: {
      email: 'test@example.com',
      password: await hash('password', 10),
      name: 'Test User',
    },
  });

  const project = await prisma.project.create({
    data: {
      name: 'Test Project',
      ownerId: user.id,
    },
  });

  return { user, project };
}
```

### 6. Mocking Strategy

Plan mocking approaches:

```typescript
// API mocking with MSW
// mocks/handlers.ts
import { rest } from 'msw';

export const handlers = [
  rest.get('/api/projects', (req, res, ctx) => {
    return res(ctx.json({
      data: projectFixtures,
      meta: { total: projectFixtures.length },
    }));
  }),

  rest.post('/api/projects', async (req, res, ctx) => {
    const body = await req.json();
    return res(ctx.status(201), ctx.json({
      id: faker.string.uuid(),
      ...body,
    }));
  }),

  rest.get('/api/projects/:id', (req, res, ctx) => {
    const { id } = req.params;
    const project = projectFixtures.find(p => p.id === id);
    if (!project) {
      return res(ctx.status(404));
    }
    return res(ctx.json(project));
  }),
];

// Module mocking
jest.mock('@/lib/storage', () => ({
  storage: {
    getItem: jest.fn(),
    setItem: jest.fn(),
    removeItem: jest.fn(),
  },
}));

// Time mocking
beforeEach(() => {
  jest.useFakeTimers();
  jest.setSystemTime(new Date('2024-01-15T12:00:00Z'));
});

afterEach(() => {
  jest.useRealTimers();
});
```

### 7. Coverage Goals

Define coverage targets:

| Area | Coverage Target | Focus |
|------|-----------------|-------|
| Business Logic | 90%+ | Critical calculations, rules |
| API Controllers | 80%+ | Request handling, validation |
| React Components | 70%+ | User interactions, states |
| Utilities | 95%+ | Pure functions |
| Integration | 100% critical paths | Auth, core flows |
| E2E | 100% critical journeys | Key user flows |

```javascript
// jest.config.js
module.exports = {
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 70,
      lines: 80,
      statements: 80,
    },
    './src/services/**/*.ts': {
      branches: 90,
      functions: 90,
      lines: 90,
    },
  },
};
```

### 8. CI/CD Integration

Plan test pipeline:

```yaml
# .github/workflows/test.yml
name: Test

on: [push, pull_request]

jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - run: npm ci
      - run: npm run test:unit -- --coverage
      
      - uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info

  integration-tests:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_DB: test
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432
    
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - run: npm ci
      - run: npm run test:integration
        env:
          DATABASE_URL: postgresql://postgres:test@localhost:5432/test

  e2e-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      
      - run: npm ci
      - run: npx playwright install --with-deps
      
      - run: npm run build
      - run: npm run test:e2e
      
      - uses: actions/upload-artifact@v3
        if: failure()
        with:
          name: playwright-report
          path: playwright-report/
```

### 9. Test Organization

Structure test files:

```
__tests__/                    # Root test directory
├── unit/                     # Unit tests
│   ├── services/
│   ├── utils/
│   └── hooks/
├── integration/              # Integration tests
│   ├── api/
│   └── components/
├── e2e/                      # E2E tests
│   ├── auth.spec.ts
│   └── projects.spec.ts
├── fixtures/                 # Test data
├── factories/                # Data factories
└── mocks/                    # Mock handlers

# Or colocated tests
src/
├── services/
│   ├── projects.service.ts
│   └── projects.service.test.ts
├── components/
│   ├── ProjectCard/
│   │   ├── ProjectCard.tsx
│   │   └── ProjectCard.test.tsx
```

## Output Format

Produce documentation with:
- Testing pyramid diagram
- Test organization structure
- Coverage goals by area
- Critical test scenarios
- Mock strategy
- CI/CD configuration
- Test data management plan

## Best Practices

- **Test behavior, not implementation**: Focus on what, not how
- **Keep tests isolated**: Each test independent
- **Use descriptive names**: Tests as documentation
- **Avoid test interdependence**: No shared state
- **Fast feedback**: Unit tests < 1s, E2E < 5min total
- **Maintain test quality**: Refactor tests too
