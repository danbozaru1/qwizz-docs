# QWZ-0.08: Configure Backend Folder Structure

## User Story

As a developer, I want a clear folder structure in the backend, so that code is organized by domain (game, matchmaking, websocket) with shared resources properly separated.

## Description

Create a lean, modern NestJS folder structure inside `backend/src/` with domain modules at the root level, shared resources in `common/`, database layer in `database/`, and environment configuration in `config/`. This structure supports real-time WebSocket features and future microservices migration without unnecessary bloat.

## Acceptance Criteria

- [ ] `backend/src/config/` folder created with subfolders:
  - `database.config.ts` (placeholder file)
  - `websocket.config.ts` (placeholder file)
- [ ] `backend/src/common/` folder created with subfolders:
  - `constants/`
  - `interfaces/`
  - `utils/`
- [ ] `backend/src/database/` folder created with subfolders:
  - `entities/`
  - `seeds/`
- [ ] `backend/src/game/` folder created with subfolder:
  - `dto/`
- [ ] `backend/src/matchmaking/` folder created with subfolder:
  - `dto/`
- [ ] `backend/src/websocket/` folder created with subfolder:
  - `gateways/`
- [ ] Each empty subfolder contains a `.gitkeep` file to preserve structure in git
- [ ] Placeholder config files created with basic TypeScript exports

## Technical Notes

```bash
cd backend/src

# Create config folder with placeholder files
mkdir config
touch config/database.config.ts
touch config/websocket.config.ts

# Create common folders
mkdir -p common/constants common/interfaces common/utils
touch common/constants/.gitkeep
touch common/interfaces/.gitkeep
touch common/utils/.gitkeep

# Create database folders
mkdir -p database/entities database/seeds
touch database/entities/.gitkeep
touch database/seeds/.gitkeep

# Create domain module folders
mkdir -p game/dto
touch game/dto/.gitkeep

mkdir -p matchmaking/dto
touch matchmaking/dto/.gitkeep

mkdir -p websocket/gateways
touch websocket/gateways/.gitkeep
```

Add placeholder content to `config/database.config.ts`:

```typescript
export const databaseConfig = {
  // Database configuration will be added in future stories
};
```

Add placeholder content to `config/websocket.config.ts`:

```typescript
export const websocketConfig = {
  // WebSocket configuration will be added in future stories
};
```

Verify structure:

```
backend/src/
├── config/
│   ├── database.config.ts
│   └── websocket.config.ts
├── common/
│   ├── constants/
│   ├── interfaces/
│   └── utils/
├── database/
│   ├── entities/
│   └── seeds/
├── game/
│   └── dto/
├── matchmaking/
│   └── dto/
├── websocket/
│   └── gateways/
├── app.controller.ts
├── app.module.ts
├── app.service.ts
└── main.ts
```

## Dependencies

QWZ-0.07 (NestJS backend must exist)

## Effort Estimate: XS
