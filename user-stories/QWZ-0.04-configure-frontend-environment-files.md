# QWZ-0.04: Configure Frontend Environment Files

## User Story

As a frontend developer, I want environment-specific configuration files so that I can easily switch between development and production API endpoints without hardcoding URLs.

## Description

Angular applications require separate configuration for development and production environments. This user story establishes the environment file structure that will be used throughout the application for API URLs, WebSocket connections, and feature flags. The Angular build system will automatically swap `environment.ts` with `environment.prod.ts` when building for production.

## Acceptance Criteria

- [ ] `frontend/src/environments/environment.ts` created with development configuration
- [ ] `frontend/src/environments/environment.prod.ts` created with production configuration
- [ ] Both environment files include `apiUrl`, `websocket`, and `features` properties
- [ ] `frontend/angular.json` configured with file replacement for production builds
- [ ] Optional: `Environment` interface created in `core/models/` for type safety
- [ ] Environment configuration verified by importing in `app.ts` and checking browser console
- [ ] Test console.log removed after verification
- [ ] Changes committed to git with message: `"configure environment files for dev/prod API URLs"`

## Technical Notes

### File Structure

```
frontend/src/
├── environments/
│   ├── environment.ts          # Development config
│   └── environment.prod.ts     # Production config
└── app/
    └── core/
        └── models/
            └── environment.model.ts  # Optional type definition
```

### Development Environment (`environment.ts`)

```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000/api',
  features: {
    enableDevTools: true,
    enableMockData: false
  },
  websocket: {
    url: 'ws://localhost:3000',
    reconnectInterval: 5000
  }
};
```

### Production Environment (`environment.prod.ts`)

```typescript
export const environment = {
  production: true,
  apiUrl: 'https://api.qwizz.app/api',
  features: {
    enableDevTools: false,
    enableMockData: false
  },
  websocket: {
    url: 'wss://api.qwizz.app',
    reconnectInterval: 5000
  }
};
```

### Optional: Environment Type Model (`core/models/environment.model.ts`)

```typescript
export interface Environment {
  production: boolean;
  apiUrl: string;
  features: {
    enableDevTools: boolean;
    enableMockData: boolean;
  };
  websocket: {
    url: string;
    reconnectInterval: number;
  };
}
```

If creating the type model, update both environment files:

```typescript
import { Environment } from '../app/core/models/environment.model';

export const environment: Environment = {
  // ... configuration
};
```

### Angular Configuration (`angular.json`)

Locate the `configurations` section under `projects.frontend.architect.build` and ensure it includes:

```json
"configurations": {
  "production": {
    "fileReplacements": [
      {
        "replace": "src/environments/environment.ts",
        "with": "src/environments/environment.prod.ts"
      }
    ],
    "optimization": true,
    "outputHashing": "all",
    "sourceMap": false,
    "namedChunks": false,
    "extractLicenses": true,
    "budgets": [
      {
        "type": "initial",
        "maximumWarning": "500kb",
        "maximumError": "1mb"
      },
      {
        "type": "anyComponentStyle",
        "maximumWarning": "2kb",
        "maximumError": "4kb"
      }
    ]
  },
  "development": {
    "optimization": false,
    "extractLicenses": false,
    "sourceMap": true
  }
}
```

### Verification Steps

1. Create environment files as specified above
2. Update `angular.json` configuration
3. Temporarily add to `app.ts`:

```typescript
import { environment } from '../environments/environment';

constructor() {
  console.log('Environment:', environment);
  console.log('API URL:', environment.apiUrl);
}
```

4. Run `npm start` from `frontend/` directory
5. Open browser console and verify output shows development configuration
6. Remove console.log statements from `app.ts`
7. Commit changes

### Commands

```bash
# Navigate to frontend
cd frontend/src

# Create environments directory
mkdir -p environments

# Create environment files (use your editor)
# Create environment.ts
# Create environment.prod.ts

# Optional: Create type model
mkdir -p app/core/models
# Create environment.model.ts

# Test
cd ../..
npm start

```

## Dependencies

- QWZ-0.03 (requires `core/models/` directory)

## Effort Estimate: XS

## Notes

- The `apiUrl` placeholder (`https://api.qwizz.app/api`) will be updated when backend is deployed
- WebSocket URL follows same pattern (http → ws, https → wss)
- Feature flags structure is prepared for future use but not required for MVP
- File replacement happens automatically during `ng build --configuration production`
