# QWZ-0.09: Configure Backend Environment Files

## User Story

As a backend developer, I want environment-specific configuration files so that I can easily switch between development and production database credentials, ports, and WebSocket settings without hardcoding values.

## Description

NestJS applications use the `@nestjs/config` package to manage environment variables across different environments. This user story establishes the environment configuration structure using `.env` files and the `ConfigModule` with namespaced configuration for database and WebSocket settings. The configuration will be type-safe and globally available throughout the application.

## Acceptance Criteria

- [ ] `@nestjs/config` package installed
- [ ] `.env.development` created in `backend/` root with development configuration
- [ ] `.env.production` created in `backend/` root with production configuration
- [ ] `ConfigModule` registered globally in `app.module.ts` with `isGlobal: true`
- [ ] `config/database.config.ts` implemented using `registerAs()` for namespaced configuration
- [ ] `config/websocket.config.ts` implemented using `registerAs()` for namespaced configuration
- [ ] Configuration loaded in `ConfigModule.forRoot()` with environment-specific `.env` file path
- [ ] `main.ts` updated to use `ConfigService` for port configuration
- [ ] Application starts successfully and reads correct environment variables
- [ ] Test console.log removed after verification

## Technical Notes

### Installation

```bash
cd backend
npm install @nestjs/config
```

### File Structure

```
backend/
├── .env.development          # Development environment variables
├── .env.production           # Production environment variables
├── src/
│   ├── config/
│   │   ├── database.config.ts    # Database configuration namespace
│   │   └── websocket.config.ts   # WebSocket configuration namespace
│   ├── app.module.ts
│   └── main.ts
└── .gitignore
```

### Development Environment (`.env.development`)

```env
NODE_ENV=development
PORT=3000

# Database
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_USER=qwizz_dev
DATABASE_PASSWORD=qwizz_dev_password
DATABASE_NAME=qwizz_dev

# WebSocket
WS_PORT=3001
WS_RECONNECT_INTERVAL=5000
```

### Production Environment (`.env.production`)

```env
NODE_ENV=production
PORT=3000

# Database
DATABASE_HOST=your-production-db-host
DATABASE_PORT=5432
DATABASE_USER=qwizz_prod
DATABASE_PASSWORD=qwizz_prod_password
DATABASE_NAME=qwizz_prod

# WebSocket
WS_PORT=3001
WS_RECONNECT_INTERVAL=5000
```

### Database Configuration (`config/database.config.ts`)

```typescript
import { registerAs } from '@nestjs/config';

export default registerAs('database', () => ({
  host: process.env.DATABASE_HOST ?? 'localhost',
  port: parseInt(process.env.DATABASE_PORT ?? '5432', 10),
  user: process.env.DATABASE_USER ?? 'qwizz_dev',
  password: process.env.DATABASE_PASSWORD ?? 'dev_password',
  name: process.env.DATABASE_NAME ?? 'qwizz_dev'
}));
```

### WebSocket Configuration (`config/websocket.config.ts`)

```typescript
import { registerAs } from '@nestjs/config';

export default registerAs('websocket', () => ({
  port: parseInt(process.env.WS_PORT ?? '3001', 10),
  reconnectInterval: parseInt(process.env.WS_RECONNECT_INTERVAL ?? '5000', 10)
}));
```

### App Module Configuration (`app.module.ts`)

```typescript
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import databaseConfig from './config/database.config';
import websocketConfig from './config/websocket.config';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: `${process.cwd()}/.env.${process.env.NODE_ENV || 'development'}`,
      load: [databaseConfig, websocketConfig]
    })
  ],
  controllers: [AppController],
  providers: [AppService]
})
export class AppModule {}
```

### Main.ts Configuration (`main.ts`)

Update `main.ts` to use `ConfigService` for port:

```typescript
import { NestFactory } from '@nestjs/core';
import { ConfigService } from '@nestjs/config';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  const configService = app.get(ConfigService);
  const port = configService.get<number>('PORT') || 3000;

  await app.listen(port);
  console.log(`Application running on port ${port}`);
}
bootstrap();
```

### Update .gitignore

Ensure `.env*` files are ignored:

```
# Add to .gitignore
.env.development
.env.production
```

### Verification Steps

1. Install `@nestjs/config` package
2. Create `.env.development` and `.env.production` files
3. Implement `database.config.ts` and `websocket.config.ts`
4. Update `app.module.ts` with `ConfigModule` configuration
5. Update `main.ts` to use `ConfigService`
6. Update `.gitignore`
7. Set `NODE_ENV` and run application:

```bash
# Test development environment
NODE_ENV=development npm run start:dev

# Verify in console that port and config are loaded correctly
```

8. Temporarily add to `app.controller.ts` to verify config loading:

```typescript
import { Controller, Get } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

@Controller()
export class AppController {
  constructor(private configService: ConfigService) {
    console.log('Database Config:', this.configService.get('database'));
    console.log('WebSocket Config:', this.configService.get('websocket'));
  }

  @Get()
  getHello(): string {
    return 'Hello World!';
  }
}
```

9. Check console output for correct configuration values
10. Remove console.log statements from `app.controller.ts`

### Commands

```bash
# Install package
cd backend
npm install @nestjs/config

# Create environment files
touch .env.development
touch .env.production

# Run in development
NODE_ENV=development npm run start:dev

# Run in production (after build)
NODE_ENV=production npm run start:prod
```

## Dependencies

QWZ-0.08 (Backend folder structure must exist)

## Effort Estimate: S

## Notes

- `NODE_ENV` must be set before running the application (via terminal or package.json scripts)
- Database credentials are placeholders - will be updated when PostgreSQL is configured
- `ConfigModule` with `isGlobal: true` makes `ConfigService` available everywhere without importing
- Namespaced configuration (`registerAs`) provides type-safe access: `configService.get('database.host')`
- `.env` files should NEVER be committed to git - add to `.gitignore` immediately
