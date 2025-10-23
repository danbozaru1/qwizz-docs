# QWZ-0.01: Initialize Monorepo Structure

## User Story

As a developer, I want a monorepo initialized with root workspace configuration, so that I can manage frontend, backend, and shared code in a single repository.

## Description

Create the root folder structure with placeholder directories for frontend, backend, docker, scripts, and shared types. Configure npm workspaces at the root level.

## Acceptance Criteria

- [ ] Root folder `qwizz-app/` created
- [ ] Subfolders created: `frontend/`, `backend/`, `docker/`, `scripts/`, `shared/`
- [ ] Root `package.json` created with workspace configuration:
  ```json
  {
    "name": "qwizz-app",
    "private": true,
    "workspaces": ["frontend"]
  }
  ```
- [ ] `.gitignore` created with standard Node.js ignores:
  ```
  node_modules/
  dist/
  .env
  .DS_Store
  ```
- [ ] Git repository initialized: `git init`
- [ ] First commit made: `git add . && git commit -m "add initial monorepo structure"`

## Technical Notes

```bash
mkdir qwizz-app
cd qwizz-app
mkdir frontend backend docker scripts shared

# Create .gitkeep files to preserve folder structure
touch frontend/.gitkeep
touch backend/.gitkeep
touch docker/.gitkeep
touch scripts/.gitkeep
touch shared/.gitkeep

# Create root package.json
npm init -y
# Edit package.json to match the configuration above

# Create .gitignore
printf "node_modules/\ndist/\n.env\n.DS_Store\n" > .gitignore

# Initialize git
git init
git add .
git commit -m "add initial monorepo structure"

# Add remote and push
git remote add origin git@github.com:danbozaru/qwizz-app.git #replace with your git url
git push -u origin main

```

## Dependencies: None

## Effort Estimate: XS
