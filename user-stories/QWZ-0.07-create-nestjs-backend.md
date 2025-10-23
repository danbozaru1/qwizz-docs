# QWZ-0.07: Create NestJS Backend

## User Story

As a developer, I want a NestJS backend initialized with TypeScript and basic module structure, so that I have a working API server ready for future implementation.

## Description

Initialize a NestJS project in the `backend/` folder with TypeScript strict mode, default module structure, and a health check endpoint. This creates the backend skeleton that will be expanded in later epics with database connections, WebSocket support, and business logic.

## Acceptance Criteria

- [ ] NestJS CLI installed globally: `npm install -g @nestjs/cli`
- [ ] NestJS project created: `nest new backend --strict --skip-git`
- [ ] TypeScript strict mode enabled in `backend/tsconfig.json`
- [ ] `main.ts` exists with `NestFactory.create()` and `app.listen(3000)`
- [ ] `app.module.ts` exists with `AppModule`
- [ ] `app.controller.ts` exists with `@Get()` decorator on default route
- [ ] `app.service.ts` exists with default service method
- [ ] `npm run start:dev` runs without errors on `localhost:3000`
- [ ] GET `http://localhost:3000` returns "Hello World!" response
- [ ] Hot reload works when editing files

## Technical Notes

```bash
# Install NestJS CLI globally (if not already installed)
npm install -g @nestjs/cli

# Navigate to qwizz-app root
cd qwizz-app

# Create NestJS project in backend/ folder
nest new backend --strict --skip-git

# Navigate to backend
cd backend

# Start development server with hot reload
npm run start:dev
```

Verify `main.ts` contains:

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

Verify `app.controller.ts` contains:

```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

Open browser at `http://localhost:3000` - should see "Hello World!" response.

## Dependencies

QWZ-0.01 (monorepo structure must exist)

## Effort Estimate: XS
