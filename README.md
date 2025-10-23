# Qwizz Documentation

Business analysis documentation for Qwizz - a real-time competitive trivia game.

This repository contains epics and user stories that define the product requirements and technical specifications. The corresponding implementation lives in [qwizz-app](https://github.com/danbozaru1/qwizz-app).

## Repository Structure

```
qwizz-docs/
├── epics/           # High-level feature epics
│   └── QWZ-0-project-foundation-monorepo.md
└── user-stories/    # Detailed user stories with acceptance criteria
    ├── QWZ-0.01-initialize-monorepo-structure.md
    ├── QWZ-0.02-create-angular-20-frontend.md
    ├── QWZ-0.03-configure-frontend-folder-structure.md
    ├── QWZ-0.04-configure-frontend-environment-files.md
    ├── QWZ-0.05-install-and-configure-ngrx.md
    ├── QWZ-0.06-configure-http-client-and-error-handling.md
    ├── QWZ-0.07-create-nestjs-backend.md
    ├── QWZ-0.08-configure-backend-folder-structure.md
    ├── QWZ-0.09-configure-backend-environment-files.md
    ├── QWZ-0.10-configure-eslint-and-prettier.md
    └── QWZ-0.11-tag-release-and-verify-success-metrics.md
```

## How to Use This Repository

Each epic is broken down into user stories. User stories contain:

- Acceptance criteria (what needs to be done)
- Technical notes (how to implement)
- Dependencies (what must exist first)
- Effort estimates (XS/S/M/L/XL)

Follow along like this:

1. Read the epic to understand the high-level goal
2. Read each user story in order
3. Implement the user story in [qwizz-app](https://github.com/danbozaru1/qwizz-app)
4. Check the corresponding git tag in qwizz-app for working code

## Linked Repositories

- **Implementation:** [qwizz-app](https://github.com/danbozaru1/qwizz-app)
- **Documentation:** [qwizz-docs](https://github.com/danbozaru1/qwizz-docs) (this repo)

## About

This documentation follows standard Business Analysis practices:

- Epics define high-level features and business value
- User stories break epics into implementable units
- Acceptance criteria ensure clear definition of done
- Technical notes provide implementation guidance

Build alongside this project to practice BA documentation and full-stack development.
