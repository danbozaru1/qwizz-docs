# QWZ-0: Project Foundation & Monorepo Setup

## Overview

Establish a full-stack monorepo structure with Angular 20 frontend, NestJS backend, and workspace configuration for a real-time competitive trivia game.

## Business Value

- **Architectural Clarity:** Monorepo structure makes full-stack development seamless from day 1
- **Code Reusability:** Shared TypeScript types between frontend/backend eliminate duplication and reduce bugs
- **Developer Experience:** Single repository, unified tooling, and clear workspace organization reduce cognitive load
- **Scalability:** Structure supports adding Docker configs and scripts without refactoring
- **Full-Stack Foundation:** Both frontend and backend working out of the box enables immediate feature development

## Technical Scope

- Initialize monorepo with root workspace configuration
- Create `frontend/` with Angular 20 standalone components
- Create `backend/` with NestJS application
- Create placeholder folders: `docker/`, `scripts/`, `shared/`
- Set up root-level linting and formatting (ESLint, Prettier)
- Configure environment files (dev/prod) for both frontend and backend
- Install and configure NgRx with functional providers in frontend
- Set up HTTP client with functional interceptors in frontend
- Configure root-level npm scripts for development workflow

## Folder Structure

```
qwizz-app/
├── frontend/              # Angular 20 app
├── backend/               # NestJS app
├── docker/                # Docker setup (future)
├── scripts/               # Database scripts (future)
├── shared/                # Shared TypeScript types (future)
├── package.json           # Root workspace config
├── .eslintrc.json
├── .prettierrc
├── .gitignore
└── README.md
```

## Dependencies

- Node.js 20+ installed
- npm 10+ package manager
- Angular CLI 20+ installed globally
- NestJS CLI installed globally

## Acceptance Criteria

### Monorepo Structure

- [ ] Root folder contains: `frontend/`, `backend/`, `docker/`, `scripts/`, `shared/`
- [ ] Root `package.json` configured with npm workspaces
- [ ] `.gitignore` configured for all workspaces
- [ ] Git repository initialized with first commit

### Frontend (Angular 20)

- [ ] Angular 20 project created with standalone components, routing, SCSS, strict mode, zoneless
- [ ] No `app.module.ts` (standalone architecture only)
- [ ] `app.config.ts` contains all providers
- [ ] `app.routes.ts` configured for routing
- [ ] Zoneless change detection enabled
- [ ] TypeScript strict mode enabled
- [ ] Frontend folder structure created (core, shared, features)
- [ ] Environment files configured (dev/prod)
- [ ] NgRx store initialized with functional providers
- [ ] HTTP client configured with base URL interceptor
- [ ] `ng serve` runs without errors
- [ ] Production build completes without errors

### Backend (NestJS)

- [ ] NestJS project created with TypeScript strict mode
- [ ] `main.ts` exists with `NestFactory.create()` and `app.listen(3000)`
- [ ] `app.module.ts` exists with `AppModule`
- [ ] `app.controller.ts` exists with `@Get()` decorator on default route
- [ ] `app.service.ts` exists with default service method
- [ ] `npm run start:dev` runs without errors on `localhost:3000`
- [ ] GET `http://localhost:3000` returns "Hello World!" response
- [ ] Hot reload works when editing files
- [ ] Backend environment files configured (dev/prod)

### Tooling

- [ ] ESLint configured at root level
- [ ] Prettier configured at root level
- [ ] Zero linting errors on fresh project
- [ ] Root scripts configured (`dev:frontend`, `dev:backend`, `build`, `lint`)

## Success Metrics

- `npm run dev:frontend` starts in <10 seconds
- `npm run dev:backend` starts in <10 seconds
- Production builds complete in <60 seconds
- Linting completes in <5 seconds
- Zero linting errors
- Frontend bundle size <500KB
- New developer can clone and run both apps in <5 minutes
- Git tagged as `qwz-0`

## Risks & Mitigations

| Risk                               | Mitigation                    |
| ---------------------------------- | ----------------------------- |
| Monorepo complexity overhead       | Keep workspace config minimal |
| Angular 20 compatibility issues    | Document exact versions       |
| NestJS module configuration issues | Use standard NestJS CLI setup |
| Workspace configuration issues     | Use standard npm workspaces   |

## Out of Scope

- Ionic/Capacitor setup
- Backend business logic implementation
- Docker configuration
- Database scripts
- Authentication/guest system
- UI components or game logic
- Testing setup
- CI/CD pipeline
