---
name: dev-code-review
description: Conduct thorough code reviews focusing on quality, security, performance, and best practices. Use when reviewing code changes, PRs, or assessing code quality.
---

# Code Review

## Purpose

Use this Skill to conduct comprehensive code reviews:
- Code quality and maintainability
- Security vulnerabilities
- Performance issues
- Best practices adherence
- Actionable improvement suggestions

## Inputs

The main agent should provide:
- Code changes (diff or files)
- Context about the feature/fix
- Any specific concerns to review
- Project coding standards (if available)

## Process

### 1. Review Categories

Organize review by category:

| Category | Priority | Focus Areas |
|----------|----------|-------------|
| Security | Critical | Auth, input validation, secrets |
| Correctness | Critical | Logic errors, edge cases |
| Performance | High | N+1 queries, memory leaks |
| Maintainability | Medium | Readability, complexity |
| Style | Low | Formatting, naming |

### 2. Security Review

Check for security issues:

```typescript
// ❌ SQL Injection
const query = `SELECT * FROM users WHERE id = ${userId}`;

// ✅ Parameterized query
const query = 'SELECT * FROM users WHERE id = $1';
await db.query(query, [userId]);

// ❌ Exposed secrets
const API_KEY = 'sk-1234567890abcdef';

// ✅ Environment variable
const API_KEY = process.env.API_KEY;

// ❌ Missing auth check
app.get('/admin/users', async (req, res) => {
  const users = await getUsers();
  res.json(users);
});

// ✅ Auth middleware
app.get('/admin/users', authMiddleware, adminOnly, async (req, res) => {
  const users = await getUsers();
  res.json(users);
});

// ❌ XSS vulnerability
element.innerHTML = userInput;

// ✅ Safe rendering
element.textContent = userInput;
// Or with sanitization
element.innerHTML = DOMPurify.sanitize(userInput);
```

### 3. Correctness Review

Check logic and edge cases:

```typescript
// ❌ Missing null check
function getFullName(user) {
  return user.firstName + ' ' + user.lastName;
}

// ✅ With null handling
function getFullName(user: User | null): string {
  if (!user) return '';
  return `${user.firstName ?? ''} ${user.lastName ?? ''}`.trim();
}

// ❌ Race condition
let data = null;
async function fetchData() {
  data = await api.getData();
}
function processData() {
  return data.map(item => item.value); // data might still be null
}

// ✅ Proper async handling
async function getData() {
  const data = await api.getData();
  return data.map(item => item.value);
}

// ❌ Off-by-one error
for (let i = 0; i <= array.length; i++) { // Should be < not <=
  process(array[i]);
}

// ✅ Correct bounds
for (let i = 0; i < array.length; i++) {
  process(array[i]);
}
// Or better: use array methods
array.forEach(item => process(item));
```

### 4. Performance Review

Identify performance issues:

```typescript
// ❌ N+1 query problem
const projects = await Project.findAll();
for (const project of projects) {
  project.owner = await User.findById(project.ownerId); // N+1 queries
}

// ✅ Eager loading
const projects = await Project.findAll({
  include: [{ model: User, as: 'owner' }],
});

// ❌ Blocking operation
function hashPassword(password: string) {
  return bcrypt.hashSync(password, 12); // Blocks event loop
}

// ✅ Async operation
async function hashPassword(password: string) {
  return bcrypt.hash(password, 12);
}

// ❌ Memory leak (React)
useEffect(() => {
  const interval = setInterval(fetchData, 5000);
  // Missing cleanup!
}, []);

// ✅ With cleanup
useEffect(() => {
  const interval = setInterval(fetchData, 5000);
  return () => clearInterval(interval);
}, []);

// ❌ Unnecessary re-renders
function ParentComponent() {
  const [count, setCount] = useState(0);
  const handleClick = () => { /* ... */ }; // New function every render
  
  return <ChildComponent onClick={handleClick} />;
}

// ✅ Memoized callback
function ParentComponent() {
  const [count, setCount] = useState(0);
  const handleClick = useCallback(() => { /* ... */ }, []);
  
  return <ChildComponent onClick={handleClick} />;
}
```

### 5. Maintainability Review

Check code quality:

```typescript
// ❌ Magic numbers
if (user.role === 1) { /* admin */ }
if (retries > 3) { throw new Error(); }

// ✅ Named constants
const ADMIN_ROLE = 1;
const MAX_RETRIES = 3;
if (user.role === ADMIN_ROLE) { /* admin */ }
if (retries > MAX_RETRIES) { throw new Error(); }

// ❌ Deep nesting
function process(data) {
  if (data) {
    if (data.items) {
      if (data.items.length > 0) {
        for (const item of data.items) {
          if (item.active) {
            // ... deep logic
          }
        }
      }
    }
  }
}

// ✅ Early returns
function process(data) {
  if (!data?.items?.length) return;
  
  const activeItems = data.items.filter(item => item.active);
  for (const item of activeItems) {
    // ... flat logic
  }
}

// ❌ Large function (>50 lines)
async function handleSubmit(data) {
  // 100+ lines of mixed concerns
}

// ✅ Extracted functions
async function handleSubmit(data) {
  const validated = await validateData(data);
  const transformed = transformForApi(validated);
  const result = await submitToApi(transformed);
  await updateLocalState(result);
  showSuccessNotification();
}
```

### 6. API Design Review

Check API patterns:

```typescript
// ❌ Inconsistent response format
// GET /users returns array
// GET /projects returns { data: array }

// ✅ Consistent envelope
interface ApiResponse<T> {
  data: T;
  meta?: PaginationMeta;
  error?: ApiError;
}

// ❌ Missing error handling
app.post('/users', async (req, res) => {
  const user = await createUser(req.body);
  res.json(user);
});

// ✅ Proper error handling
app.post('/users', async (req, res, next) => {
  try {
    const user = await createUser(req.body);
    res.status(201).json({ data: user });
  } catch (error) {
    next(error);
  }
});

// ❌ Exposing internal errors
catch (error) {
  res.status(500).json({ error: error.message, stack: error.stack });
}

// ✅ Safe error response
catch (error) {
  logger.error('User creation failed', { error, requestId: req.id });
  res.status(500).json({
    error: {
      code: 'INTERNAL_ERROR',
      message: 'An unexpected error occurred',
      requestId: req.id,
    },
  });
}
```

### 7. React/Component Review

Check component patterns:

```typescript
// ❌ Direct DOM manipulation
function Component() {
  document.getElementById('myDiv').style.display = 'none';
  return <div id="myDiv">Content</div>;
}

// ✅ React state
function Component() {
  const [isVisible, setIsVisible] = useState(true);
  return isVisible ? <div>Content</div> : null;
}

// ❌ Props drilling
<GrandParent user={user}>
  <Parent user={user}>
    <Child user={user}>
      <GrandChild user={user} />
    </Child>
  </Parent>
</GrandParent>

// ✅ Context or composition
const UserContext = createContext<User | null>(null);

<UserContext.Provider value={user}>
  <GrandParent>
    <Parent>
      <Child>
        <GrandChild /> {/* Uses useContext */}
      </Child>
    </Parent>
  </GrandParent>
</UserContext.Provider>

// ❌ Side effects in render
function Component({ id }) {
  const data = fetchData(id); // Called every render!
  return <div>{data}</div>;
}

// ✅ Effects for side effects
function Component({ id }) {
  const [data, setData] = useState(null);
  useEffect(() => {
    fetchData(id).then(setData);
  }, [id]);
  return <div>{data}</div>;
}
```

### 8. Review Feedback Format

Structure feedback clearly:

```markdown
## Code Review: Feature XYZ

### 🔴 Critical (Must Fix)

**Security: SQL Injection Risk**
File: `src/services/users.service.ts:45`
```typescript
// Current
const query = `SELECT * FROM users WHERE email = '${email}'`;

// Suggested
const query = 'SELECT * FROM users WHERE email = $1';
await db.query(query, [email]);
```
SQL injection vulnerability. User input must be parameterized.

---

### 🟠 Important (Should Fix)

**Performance: N+1 Query**
File: `src/services/projects.service.ts:78`
Consider eager loading the owner relationship to avoid N+1 queries.
See: [documentation link]

---

### 🟡 Suggestions (Consider)

**Maintainability: Extract Function**
File: `src/controllers/auth.controller.ts:120-180`
This function is 60 lines. Consider extracting validation and
transformation logic into separate functions.

---

### ✅ Good Practices Observed
- Proper TypeScript types throughout
- Good error handling in API routes
- Clear naming conventions
```

## Output Format

Produce review with:
- Summary (approve/request changes/comment)
- Critical issues (must fix)
- Important issues (should fix)
- Suggestions (nice to have)
- Positive observations
- Links to relevant documentation

## Best Practices

- **Be specific**: Point to exact lines and provide examples
- **Explain why**: Help the author understand the issue
- **Offer solutions**: Don't just criticize, suggest fixes
- **Prioritize**: Focus on important issues first
- **Be constructive**: Review code, not the person
- **Acknowledge good work**: Highlight positive patterns
