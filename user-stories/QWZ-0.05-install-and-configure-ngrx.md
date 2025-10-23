# QWZ-0.05: Install and Configure NgRx

## User Story

As a developer, I want NgRx store configured with initial state structure so that I have centralized state management ready for game features.

## Description

Configure NgRx with a basic store structure that will support game state and UI state. This establishes the foundation for reactive state management throughout the application.

The store should be organized by feature domains (game, UI) with each feature having its own actions, reducers, selectors, and effects files. Redux DevTools should be enabled for debugging.

## Acceptance Criteria

- [ ] NgRx packages installed (`@ngrx/store`, `@ngrx/effects`, `@ngrx/store-devtools`)
- [ ] Root store configured in `app.config.ts` using standalone API with zoneless change detection
- [ ] Store folder structure created with feature-based organization: `game/`, `ui/`
- [ ] Each feature module has: `actions.ts`, `reducer.ts`, `selectors.ts`, `effects.ts`
- [ ] Initial state interfaces defined for each feature
- [ ] Root `app.state.ts` combines all feature states
- [ ] Store initializes successfully with default state
- [ ] Redux DevTools extension works in browser and shows initial state

## Technical Notes

### Installation

Install required NgRx packages:

```bash
npm install @ngrx/store @ngrx/effects @ngrx/store-devtools
```

### Folder Structure

Create store organization under `frontend/src/app/store/`:

```
store/
├── app.state.ts
├── game/
│   ├── game.actions.ts
│   ├── game.reducer.ts
│   ├── game.selectors.ts
│   └── game.effects.ts
└── ui/
    ├── ui.actions.ts
    ├── ui.reducer.ts
    ├── ui.selectors.ts
    └── ui.effects.ts
```

### State Structure

**Game State** is currently empty, it will be set up after the database schema is designed and the contracts are settled.

**UI State** should track:

- `loading: boolean` - Global loading indicator
- `error: string | null` - Error messages to display
- `modalOpen: boolean` - Modal visibility state

### Implementation Guidelines

**Root State (`app.state.ts`):**

- Import all feature state interfaces
- Export combined `AppState` interface

**Reducers:**

- Use `createReducer()` from `@ngrx/store`
- Define and export state interface
- Define and export initial state constant
- Leave action handlers empty for now (will be added in feature stories)

**Selectors:**

- Use `createFeatureSelector()` to create base feature selector
- Additional selectors will be added in feature stories

**Effects:**

- Create empty `@Injectable()` class
- Inject `Actions` from `@ngrx/effects`
- Effect logic will be added in feature stories

**Actions:**

- Leave files with comment: "Actions will be added in future stories"

**Store Configuration (`app.config.ts`):**

- Use `provideZonelessChangeDetection()` for Angular 19+ zoneless mode
- Use `provideStore()` with feature reducer map
- Use `provideStoreDevtools()` with `logOnly: environment.production` to disable in production
- Use `provideEffects()` with empty effects array
- Import environment from `../environments/environment`

### Verification Steps

1. Run `npm start` in frontend directory
2. Open browser to `http://localhost:4200`
3. Open Redux DevTools browser extension
4. Verify state tree shows 2 feature slices: `game`, `ui`
5. Verify each slice shows correct initial state structure
6. Verify no console errors

## Dependencies

- QWZ-0.02 (Angular 20 frontend must exist)
- QWZ-0.03 (Frontend folder structure must be configured)
- QWZ-0.04 (Frontend environment files must be configured)

## Effort Estimate: S

## Notes

- Actions and effects remain empty - they'll be populated in feature-specific stories
- Focus on infrastructure setup, not business logic
- Store Devtools Logger not needed - will cause performance issues with 50ms game updates
