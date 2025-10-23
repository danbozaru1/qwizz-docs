# QWZ-0.02: Create Angular 20 Frontend

## User Story

As a developer, I want an Angular 20 project with standalone components and zoneless change detection, so that I have a modern, performant foundation for the frontend.

## Description

Initialize Angular 20 project in the `frontend/` folder with standalone components, routing, SCSS, strict mode, and zoneless change detection.

## Acceptance Criteria

- [ ] Angular CLI 20+ installed globally: `npm install -g @angular/cli@latest`
- [ ] Angular project created: `ng new frontend --standalone --routing --style=scss --strict --zoneless --prefix=qwizz --skip-git`
- [ ] No `app.module.ts` exists (standalone architecture)
- [ ] `app.config.ts` exists with providers array
- [ ] `app.routes.ts` exists for routing configuration
- [ ] `provideZonelessChangeDetection()` present in `app.config.ts`
- [ ] TypeScript strict mode enabled in `frontend/tsconfig.json`
- [ ] `ng serve` runs without errors on `localhost:4200`
- [ ] Default Angular welcome page displays

## Technical Notes

```bash
# Install Angular CLI globally (if not already installed)
npm install -g @angular/cli@latest

# Navigate to qwizz-app root
cd qwizz-app

# Create Angular project in frontend/ folder (will install npm dependencies automatically)
ng new frontend --standalone --routing --style=scss --strict --zoneless --prefix=qwizz --skip-git

# Navigate to frontend
cd frontend

# Serve the app
ng serve
```

Verify `app.config.ts` contains:

```typescript
import { ApplicationConfig, provideZonelessChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideZonelessChangeDetection(), provideRouter(routes)]
};
```

Open browser at `http://localhost:4200` - should see Angular welcome page.

## Dependencies

QWZ-0.01 (monorepo structure must exist)

## Effort Estimate: XS
