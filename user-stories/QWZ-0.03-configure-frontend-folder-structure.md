# QWZ-0.03: Configure Frontend Folder Structure

## User Story

As a developer, I want a clear folder structure in the frontend, so that code is organized by concern (core, shared, features).

## Description

Create `core/`, `shared/`, and `features/` folders inside `frontend/src/app/` with appropriate subfolders for services, components, etc.

## Acceptance Criteria

- [ ] `frontend/src/app/core/` folder created with subfolders:
  - `services/`
  - `constants/`
  - `models/`
  - `interceptors/`
- [ ] `frontend/src/app/shared/` folder created with subfolders:
  - `components/`
  - `pipes/`
  - `directives/`
- [ ] `frontend/src/app/features/` folder created (empty for now)
- [ ] Each subfolder contains a `.gitkeep` file to preserve structure in git

## Technical Notes

```bash
cd frontend/src/app

# Create core folders
mkdir -p core/services core/constants core/models core/interceptors

# Create shared folders
mkdir -p shared/components shared/pipes shared/directives

# Create features folder
mkdir features

# Create .gitkeep files
touch core/services/.gitkeep
touch core/constants/.gitkeep
touch core/models/.gitkeep
touch core/interceptors/.gitkeep
touch shared/components/.gitkeep
touch shared/pipes/.gitkeep
touch shared/directives/.gitkeep
touch features/.gitkeep
```

Verify structure:

```
frontend/src/app/
├── core/
│   ├── services/
│   ├── constants/
│   ├── models/
│   └── interceptors/
├── shared/
│   ├── components/
│   ├── pipes/
│   └── directives/
├── features/
├── app.component.ts
├── app.config.ts
└── app.routes.ts
```

## Dependencies

QWZ-0.02 (Angular project must exist)

## Effort Estimate: XS
