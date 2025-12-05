---
name: dev-mobile-implementation
description: Plan mobile app implementation with navigation, native features, and cross-platform patterns. Use when creating detailed mobile development plans for React Native, Flutter, or native platforms.
---

# Mobile Implementation Planning

## Purpose

Use this Skill to create detailed mobile implementation plans:
- App architecture and navigation
- Component/widget design
- Native feature integration
- Platform-specific considerations
- Implementation task breakdown

## Inputs

The main agent should provide:
- UI/UX design specifications
- API specifications
- Platform requirements (iOS, Android, or both)
- Required native features

## Process

### 1. Project Structure (React Native/Expo)

```
src/
├── app/                      # App entry and navigation
│   ├── _layout.tsx          # Root layout (Expo Router)
│   ├── index.tsx            # Entry point
│   ├── (auth)/              # Auth screens group
│   │   ├── login.tsx
│   │   └── register.tsx
│   └── (main)/              # Main app screens
│       ├── _layout.tsx      # Tab navigator
│       ├── home/
│       ├── projects/
│       └── settings/
│
├── components/
│   ├── ui/                  # Base UI components
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── Card.tsx
│   ├── features/            # Feature components
│   │   ├── projects/
│   │   └── tasks/
│   └── layouts/             # Layout components
│       └── ScreenLayout.tsx
│
├── lib/
│   ├── api/                 # API client
│   ├── storage/             # Async storage helpers
│   └── utils/               # Helpers
│
├── hooks/                   # Custom hooks
│   ├── useAuth.ts
│   └── useProjects.ts
│
├── stores/                  # State management
│   └── authStore.ts
│
├── types/                   # TypeScript types
│
└── constants/               # App constants
    ├── colors.ts
    └── spacing.ts
```

### 2. Navigation Architecture

Design navigation structure:

```typescript
// Navigation hierarchy (Expo Router)
app/
├── _layout.tsx              // Root: Auth check, providers
├── index.tsx                // Redirect based on auth
│
├── (auth)/                  // Public routes (no auth)
│   ├── _layout.tsx          // Stack navigator
│   ├── login.tsx
│   ├── register.tsx
│   └── forgot-password.tsx
│
└── (main)/                  // Protected routes
    ├── _layout.tsx          // Tab navigator
    │
    ├── home/
    │   ├── _layout.tsx      // Stack for home
    │   └── index.tsx
    │
    ├── projects/
    │   ├── _layout.tsx      // Stack for projects
    │   ├── index.tsx        // Project list
    │   └── [id].tsx         // Project detail
    │
    └── settings/
        ├── _layout.tsx
        ├── index.tsx
        └── profile.tsx

// Deep linking configuration
const linking = {
  prefixes: ['myapp://', 'https://myapp.com'],
  config: {
    screens: {
      Main: {
        screens: {
          Projects: {
            screens: {
              ProjectDetail: 'projects/:id',
            },
          },
        },
      },
    },
  },
};
```

### 3. Component Design

Define core components:

```typescript
// Screen component pattern
interface ProjectDetailScreenProps {
  route: RouteProp<ProjectStackParams, 'ProjectDetail'>;
}

function ProjectDetailScreen({ route }: ProjectDetailScreenProps) {
  const { id } = route.params;
  const { data, isLoading, error } = useProject(id);

  if (isLoading) return <ScreenSkeleton />;
  if (error) return <ErrorScreen error={error} />;

  return (
    <ScreenLayout>
      <ScrollView>
        <ProjectHeader project={data} />
        <ProjectTabs project={data} />
      </ScrollView>
    </ScreenLayout>
  );
}

// Base component props pattern
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'outline' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  loading?: boolean;
  disabled?: boolean;
  onPress: () => void;
  children: React.ReactNode;
}
```

### 4. State Management

Plan state organization:

```typescript
// Server state: TanStack Query
const useProjects = (filters: ProjectFilters) => {
  return useQuery({
    queryKey: ['projects', filters],
    queryFn: () => api.projects.list(filters),
    staleTime: 5 * 60 * 1000,
  });
};

// Client state: Zustand + MMKV
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';
import { storage } from '@/lib/storage';

interface AuthState {
  user: User | null;
  token: string | null;
  setAuth: (user: User, token: string) => void;
  logout: () => void;
}

const useAuthStore = create<AuthState>()(
  persist(
    (set) => ({
      user: null,
      token: null,
      setAuth: (user, token) => set({ user, token }),
      logout: () => set({ user: null, token: null }),
    }),
    {
      name: 'auth-storage',
      storage: createJSONStorage(() => storage),
    }
  )
);
```

### 5. Native Features Integration

Plan native capabilities:

```typescript
// Push Notifications (Expo)
import * as Notifications from 'expo-notifications';

async function registerForPushNotifications() {
  const { status } = await Notifications.requestPermissionsAsync();
  if (status !== 'granted') return null;
  
  const token = await Notifications.getExpoPushTokenAsync();
  return token.data;
}

// Biometric Authentication
import * as LocalAuthentication from 'expo-local-authentication';

async function authenticateWithBiometrics() {
  const hasHardware = await LocalAuthentication.hasHardwareAsync();
  const supportedTypes = await LocalAuthentication.supportedAuthenticationTypesAsync();
  
  const result = await LocalAuthentication.authenticateAsync({
    promptMessage: 'Authenticate to continue',
    fallbackLabel: 'Use password',
  });
  
  return result.success;
}

// Camera/Image Picker
import * as ImagePicker from 'expo-image-picker';

async function pickImage() {
  const result = await ImagePicker.launchImageLibraryAsync({
    mediaTypes: ImagePicker.MediaTypeOptions.Images,
    allowsEditing: true,
    aspect: [1, 1],
    quality: 0.8,
  });
  
  if (!result.canceled) {
    return result.assets[0];
  }
}
```

### 6. Offline Support

Design offline-first patterns:

```typescript
// Offline data with TanStack Query
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      networkMode: 'offlineFirst',
      staleTime: Infinity,
      gcTime: 1000 * 60 * 60 * 24, // 24 hours
    },
    mutations: {
      networkMode: 'offlineFirst',
    },
  },
});

// Persist cache
import { createAsyncStoragePersister } from '@tanstack/query-async-storage-persister';

const persister = createAsyncStoragePersister({
  storage: AsyncStorage,
});

// Optimistic updates
const createProjectMutation = useMutation({
  mutationFn: api.projects.create,
  onMutate: async (newProject) => {
    await queryClient.cancelQueries(['projects']);
    const previous = queryClient.getQueryData(['projects']);
    
    queryClient.setQueryData(['projects'], (old) => ({
      ...old,
      data: [...old.data, { ...newProject, id: 'temp' }],
    }));
    
    return { previous };
  },
  onError: (err, _, context) => {
    queryClient.setQueryData(['projects'], context.previous);
  },
  onSettled: () => {
    queryClient.invalidateQueries(['projects']);
  },
});
```

### 7. Platform-Specific Code

Handle platform differences:

```typescript
// Platform-specific styles
import { Platform, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  shadow: {
    ...Platform.select({
      ios: {
        shadowColor: '#000',
        shadowOffset: { width: 0, height: 2 },
        shadowOpacity: 0.1,
        shadowRadius: 4,
      },
      android: {
        elevation: 4,
      },
    }),
  },
});

// Platform-specific components
const DatePicker = Platform.select({
  ios: () => require('./DatePickerIOS').default,
  android: () => require('./DatePickerAndroid').default,
})!;

// Platform-specific behavior
function handleShare() {
  if (Platform.OS === 'ios') {
    Share.share({ url: shareUrl });
  } else {
    Share.share({ message: shareUrl });
  }
}
```

### 8. Performance Optimization

Plan for performance:

```typescript
// List optimization with FlashList
import { FlashList } from '@shopify/flash-list';

<FlashList
  data={projects}
  renderItem={({ item }) => <ProjectCard project={item} />}
  estimatedItemSize={100}
  keyExtractor={(item) => item.id}
/>

// Memoization
const ProjectCard = memo(function ProjectCard({ project }: Props) {
  return (
    <Pressable onPress={() => navigate('ProjectDetail', { id: project.id })}>
      {/* Card content */}
    </Pressable>
  );
});

// Image optimization
import { Image } from 'expo-image';

<Image
  source={{ uri: imageUrl }}
  placeholder={blurhash}
  contentFit="cover"
  transition={200}
/>
```

### 9. Testing Strategy

Plan test approach:

```typescript
// Component tests with React Native Testing Library
describe('ProjectCard', () => {
  it('renders project name', () => {
    const project = createMockProject();
    render(<ProjectCard project={project} />);
    expect(screen.getByText(project.name)).toBeTruthy();
  });

  it('navigates on press', () => {
    const project = createMockProject();
    render(<ProjectCard project={project} />);
    fireEvent.press(screen.getByRole('button'));
    expect(mockNavigate).toHaveBeenCalledWith('ProjectDetail', { id: project.id });
  });
});

// E2E tests with Detox
describe('Login flow', () => {
  beforeAll(async () => {
    await device.launchApp();
  });

  it('should login successfully', async () => {
    await element(by.id('email-input')).typeText('test@example.com');
    await element(by.id('password-input')).typeText('password');
    await element(by.id('login-button')).tap();
    await expect(element(by.id('home-screen'))).toBeVisible();
  });
});
```

### 10. Implementation Tasks

| Task | Priority | Estimate | Dependencies |
|------|----------|----------|--------------|
| Setup Expo project | High | 2h | - |
| Configure navigation | High | 4h | Setup |
| Setup API client | High | 3h | Setup |
| Implement auth flow | High | 8h | Navigation, API |
| Build tab navigation | High | 4h | Auth |
| Build project list | High | 6h | Tabs |
| Build project detail | High | 6h | Project list |
| Add push notifications | Medium | 4h | Auth |
| Add offline support | Medium | 6h | API client |
| Platform-specific polish | Medium | 4h | All screens |
| Performance optimization | Medium | 4h | All components |
| Write tests | Low | 8h | All screens |

## Output Format

Produce documentation with:
- Project structure diagram
- Navigation flow diagram
- Component specifications
- State management plan
- Native features implementation
- Platform considerations
- Task breakdown with estimates

## Best Practices

- **Expo managed workflow**: Easier updates and native features
- **Offline-first**: Plan for connectivity issues
- **Performance**: Use FlashList, memoization, image optimization
- **Platform awareness**: Respect iOS and Android conventions
- **Accessibility**: Support screen readers and dynamic type
