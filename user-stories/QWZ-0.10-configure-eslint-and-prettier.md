# QWZ-0.10: Configure ESLint and Prettier

## User Story

As a developer, I want ESLint and Prettier configured across the monorepo so that code quality is enforced and formatting is consistent between frontend and backend.

## Description

Configure ESLint and Prettier for both frontend (Angular) and backend (NestJS) with shared formatting rules. ESLint enforces code quality standards, while Prettier handles automatic code formatting. Both tools are configured to work together without conflicts, and npm scripts are added for linting and formatting.

## Acceptance Criteria

- [ ] `prettier` and `eslint-config-prettier` installed in root `package.json`
- [ ] `.prettierrc` created in monorepo root with shared formatting rules
- [ ] Frontend ESLint configured with Angular-specific rules and Prettier integration
- [ ] Backend ESLint configured with NestJS-specific rules and Prettier integration
- [ ] Root `package.json` includes lint and format scripts for both frontend and backend
- [ ] `npm run lint` runs ESLint on both frontend and backend
- [ ] `npm run lint:fix` runs ESLint with `--fix` on both frontend and backend
- [ ] `npm run format` runs Prettier on both frontend and backend
- [ ] `npm run format:check` verifies formatting without modifying files
- [ ] No ESLint or Prettier conflicts exist
- [ ] All scripts run without errors

## Technical Notes

### Installation

```bash
# Install Prettier and ESLint config in root
npm install --save-dev prettier eslint-config-prettier

# Frontend: Angular ESLint (if not already installed)
cd frontend
ng add angular-eslint

# Backend: NestJS comes with ESLint and Prettier preinstalled
```

### Prettier Configuration (`.prettierrc` in monorepo root)

Move `backend/.prettierrc` to `qwizz-app/.prettierrc` and expand with additional rules:

```json
{
  "singleQuote": true,
  "semi": true,
  "trailingComma": "es5",
  "printWidth": 147,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

**Remove Prettier config from `frontend/package.json`** if it exists:

```json
"prettier": {
  "printWidth": 100,
  "singleQuote": true,
  "overrides": [
    {
      "files": "*.html",
      "options": {
        "parser": "angular"
      }
    }
  ]
}
```

### Frontend ESLint Configuration

Frontend already has `eslint.config.js` from `ng add angular-eslint`. Update it to include `eslint-config-prettier` and ignore build artifacts:

```javascript
// frontend/eslint.config.js
const eslint = require('@eslint/js');
const tseslint = require('typescript-eslint');
const angular = require('angular-eslint');
const eslintConfigPrettier = require('eslint-config-prettier');

module.exports = tseslint.config(
  {
    ignores: ['.angular/**', 'dist/**', 'node_modules/**']
  },
  {
    files: ['**/*.ts'],
    extends: [
      eslint.configs.recommended,
      ...tseslint.configs.recommended,
      ...tseslint.configs.stylistic,
      ...angular.configs.tsRecommended,
      eslintConfigPrettier
    ],
    processor: angular.processInlineTemplates,
    rules: {
      '@angular-eslint/directive-selector': [
        'error',
        {
          type: 'attribute',
          prefix: 'qwizz',
          style: 'camelCase'
        }
      ],
      '@angular-eslint/component-selector': [
        'error',
        {
          type: 'element',
          prefix: 'qwizz',
          style: 'kebab-case'
        }
      ],
      '@angular-eslint/contextual-lifecycle': 'off'
    }
  },
  {
    files: ['**/*.html'],
    extends: [...angular.configs.templateRecommended, ...angular.configs.templateAccessibility],
    rules: {}
  }
);
```

**Key changes:**

- Added `ignores` block at top to exclude `.angular/`, `dist/`, and `node_modules/`
- Disabled deprecated `@angular-eslint/contextual-lifecycle` rule

### Backend ESLint Configuration

Backend already has `eslint.config.mjs` with Prettier plugin preinstalled from NestJS CLI. Update ignores to exclude itself (`eslint.config.mjs`), `dist/`, and `node_modules`:

```javascript
// backend/eslint.config.mjs
import eslint from '@eslint/js';
import eslintPluginPrettierRecommended from 'eslint-plugin-prettier/recommended';
import globals from 'globals';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  {
    ignores: ['eslint.config.mjs', 'dist/**', 'node_modules/**']
  },
  eslint.configs.recommended,
  ...tseslint.configs.recommendedTypeChecked,
  eslintPluginPrettierRecommended,
  {
    languageOptions: {
      globals: {
        ...globals.node,
        ...globals.jest
      },
      sourceType: 'commonjs',
      parserOptions: {
        projectService: true,
        tsconfigRootDir: import.meta.dirname
      }
    }
  },
  {
    rules: {
      '@typescript-eslint/no-explicit-any': 'off',
      '@typescript-eslint/no-floating-promises': 'warn',
      '@typescript-eslint/no-unsafe-argument': 'warn',
      'prettier/prettier': ['error', { endOfLine: 'auto' }]
    }
  }
);
```

**Key changes:**

- Added `eslint.config.mjs`, `dist/**` and `node_modules/**` to ignores

### Root Package.json Scripts

Add to root `package.json`:

```json
{
  "scripts": {
    "lint": "npm run lint --workspaces",
    "lint:fix": "npm run lint:fix --workspaces",
    "format": "prettier --write \"frontend/src/**/*.{ts,html,scss}\" \"backend/src/**/*.ts\"",
    "format:check": "prettier --check \"frontend/src/**/*.{ts,html,scss}\" \"backend/src/**/*.ts\""
  }
}
```

### Frontend Package.json Scripts

Ensure `frontend/package.json` has:

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  }
}
```

### Backend Package.json Scripts

Backend already has lint and format scripts from NestJS CLI. Update to match frontend pattern:

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "format": "prettier --write \"src/**/*.ts\" \"test/**/*.ts\""
  }
}
```

### Verification Steps

1. Install Prettier and eslint-config-prettier in root
2. Move `backend/.prettierrc` to `qwizz-app/.prettierrc` and expand with additional rules
3. Update frontend `eslint.config.js` with ignores and disabled deprecated rule
4. Update backend `eslint.config.mjs` with ignores
5. Update root and workspace `package.json` scripts
6. Run verification:

```bash
# Check formatting
npm run format:check

# Lint both frontend and backend
npm run lint

# Auto-fix linting issues
npm run lint:fix

# Auto-format code
npm run format
```

7. Verify no errors or conflicts

## Dependencies

QWZ-0.04 (Frontend environment files)
QWZ-0.09 (Backend environment files)

## Effort Estimate

M

## Notes

- ESLint and Prettier are configured to work together without conflicts via `eslint-config-prettier`
- Prettier handles all formatting; ESLint focuses on code quality
- Monorepo scripts use `--workspaces` flag to run across all packages
- Frontend uses flat config format (`eslint.config.js`), backend uses ESM flat config (`eslint.config.mjs`)
- `.prettierrc` in root applies to entire monorepo
- NestJS CLI pre-installs ESLint and Prettier with sensible defaults
- Angular CLI requires `ng add angular-eslint` for ESLint support
