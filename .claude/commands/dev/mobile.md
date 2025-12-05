---
description: Create mobile implementation plan (React Native or Flutter) for an MVP
argument-hint: <mvp-slug> [react-native|flutter]
allowed-tools: Bash(mkdir:*), Write(*), Read(*)
---

# Mobile Implementation Plan

## Context

- MVP Slug & Framework: $ARGUMENTS
- Config: !`cat workspaces/$(echo $ARGUMENTS | awk '{print $1}')/config.json 2>/dev/null || echo "MVP_NOT_FOUND"`
- Architecture: !`cat workspaces/$(echo $ARGUMENTS | awk '{print $1}')/code/architecture/architecture.md 2>/dev/null || echo ""`
- Hi-Fi Specs: !`cat workspaces/$(echo $ARGUMENTS | awk '{print $1}')/ui-ux/high-fidelity/high-fidelity-specs.md 2>/dev/null || echo ""`
- Timestamp: !`date +%Y-%m-%d`

## Instructions

You are creating a **Mobile Implementation Plan** for the MVP specified above.

Parse the arguments:
- First argument: MVP slug
- Second argument (optional): Framework - `react-native` (default) or `flutter`

### Pre-check

1. **Verify MVP exists**: Check if config was loaded successfully
2. If "MVP_NOT_FOUND", inform user to run `/start-mvp` first
3. Determine framework (default: react-native)

---

## React Native Plan (if framework = react-native)

Use the `react-native-developer` agent.

### Project Structure
```
src/
├── app/                # Expo Router
├── components/         # React Native components
├── screens/           # Screen components
├── navigation/        # Navigation config
├── hooks/             # Custom hooks
├── stores/            # State management
├── services/          # API services
└── utils/             # Utilities
```

### Key Sections
1. Navigation structure (React Navigation)
2. Screen breakdown
3. Component architecture
4. Native feature integration
5. State management (Zustand + React Query)
6. API integration
7. Offline support strategy
8. Testing approach
9. App store preparation

---

## Flutter Plan (if framework = flutter)

Use the `flutter-developer` agent.

### Project Structure
```
lib/
├── app/               # App configuration
├── core/              # Core utilities
├── features/          # Feature modules
├── shared/            # Shared widgets
└── main.dart
```

### Key Sections
1. Navigation (GoRouter)
2. Screen/Page breakdown
3. Widget architecture
4. State management (Riverpod/BLoC)
5. API integration (Dio + Retrofit)
6. Platform-specific adaptations
7. Testing approach
8. App store preparation

---

### Output

1. Create mobile plan at:
   `workspaces/{mvp-slug}/code/mobile/mobile-plan.md`

2. Create screen spec at:
   `workspaces/{mvp-slug}/code/mobile/screen-spec.md`

3. Create navigation structure at:
   `workspaces/{mvp-slug}/code/mobile/navigation.md`

4. Update `workspaces/{mvp-slug}/config.json`:
   - Add artifact paths to `artifacts` array
   - Update `updatedAt` timestamp

5. Display summary:
   ```
   ✅ Mobile plan complete for {mvp-name}
   📁 Output: workspaces/{mvp-slug}/code/mobile/
   
   Framework: {react-native|flutter}
   
   Development phase planning complete!
   ```
