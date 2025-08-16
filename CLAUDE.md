# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Core Development
- `pnpm dev` - Start development servers (backend + frontend) in parallel
- `pnpm dev:be` - Start backend-only development
- `pnpm dev:fe` - Start frontend-only development
- `pnpm build` - Build all packages using Turbo
- `pnpm build:backend` - Build backend packages
- `pnpm build:frontend` - Build frontend packages
- `pnpm build:nodes` - Build node packages

### Testing
- `pnpm test` - Run all tests
- `pnpm test:backend` - Run backend tests (SQLite by default)
- `pnpm test:frontend` - Run frontend tests
- `pnpm test:nodes` - Run node tests
- `pnpm test:postgres` - Run backend tests with PostgreSQL
- `pnpm test:mysql` - Run backend tests with MySQL
- `pnpm test:mariadb` - Run backend tests with MariaDB

### Code Quality
- `pnpm lint` - Run ESLint on all packages
- `pnpm lintfix` - Fix ESLint issues automatically
- `pnpm format` - Format code using Biome
- `pnpm format:check` - Check code formatting
- `pnpm typecheck` - Run TypeScript type checking

### E2E Testing
- `pnpm dev:e2e` - Run E2E tests in development mode
- `cd cypress && pnpm run test:e2e:dev` - Alternative E2E command

### Running n8n
- `pnpm start` - Start n8n CLI (uses packages/cli/bin/n8n)
- `pnpm start:tunnel` - Start n8n with tunneling
- `pnpm webhook` - Run webhook worker
- `pnpm worker` - Run queue worker

## Architecture Overview

n8n is a monorepo using **Turbo** for build orchestration and **pnpm** for package management. The architecture consists of:

### Core Backend Packages
- **packages/cli/** - Main CLI application and Express server
- **packages/core/** - Core workflow execution engine
- **packages/workflow/** - Workflow definition and processing logic
- **packages/nodes-base/** - Base node implementations (400+ integrations)

### Frontend Packages
- **packages/frontend/editor-ui/** - Main Vue.js workflow editor UI
- **packages/frontend/@n8n/design-system/** - Shared Vue components and design system
- **packages/frontend/@n8n/stores/** - Pinia stores for state management
- **packages/frontend/@n8n/composables/** - Vue composables

### Shared Infrastructure
- **packages/@n8n/db/** - Database abstractions and TypeORM configuration
- **packages/@n8n/api-types/** - TypeScript types for API contracts
- **packages/@n8n/permissions/** - Role-based access control
- **packages/@n8n/task-runner/** - Background task execution

### Node Development
- **packages/node-dev/** - CLI tools for node development
- **packages/@n8n/nodes-langchain/** - LangChain AI integration nodes
- **packages/@n8n/extension-sdk/** - SDK for building extensions

## Key Development Patterns

### Backend Structure
- Main application entry point: `packages/cli/src/index.ts`
- Server configuration: `packages/cli/src/server.ts`
- Workflow execution: `packages/cli/src/workflow-runner.ts`
- Node loading: `packages/cli/src/load-nodes-and-credentials.ts`

### Frontend Structure
- Main Vue app: `packages/frontend/editor-ui/src/App.vue`
- Router: `packages/frontend/editor-ui/src/router.ts`
- Stores: `packages/frontend/editor-ui/src/stores/`
- Components: `packages/frontend/editor-ui/src/components/`

### Node Development
- Nodes are located in `packages/nodes-base/nodes/`
- Each node has: `.node.ts` (logic), `.node.json` (metadata), and descriptions
- Use `packages/node-dev/bin/cli.js new` to create new nodes

### Testing Strategy
- Backend: Jest with SQLite as default database
- Frontend: Vitest with Vue Test Utils
- E2E: Cypress with real browser testing
- Node tests: Isolated testing with mock credentials

## Technology Stack

### Backend
- **Node.js** 22.16+ (Express.js server)
- **TypeScript** for type safety
- **TypeORM** with support for SQLite, PostgreSQL, MySQL, MariaDB
- **Bull** for queue processing
- **Redis** for caching and sessions
- **Jest** for testing

### Frontend
- **Vue 3** with Composition API
- **TypeScript** for type safety
- **Pinia** for state management
- **Vue Router** for routing
- **Element Plus** for UI components
- **Vue Flow** for workflow canvas
- **CodeMirror** for code editing
- **Vite** for build tooling

### Build System
- **Turbo** for monorepo management
- **pnpm** for package management
- **Biome** for formatting and linting
- **ESLint** for additional linting rules

## Environment Setup

1. Install Node.js 22.16+ and pnpm 10.2.1+
2. Run `pnpm install` to install dependencies
3. Use `pnpm dev` to start development servers
4. Access the UI at http://localhost:8080 and backend at http://localhost:5678

## Database Configuration

The application supports multiple databases:
- **SQLite** (default for development)
- **PostgreSQL** 
- **MySQL**
- **MariaDB**

Database configuration can be set via environment variables:
- `DB_TYPE` - Database type
- `DB_POSTGRESDB_SCHEMA` - PostgreSQL schema
- `DB_TABLE_PREFIX` - Table prefix for testing

## Important Notes

- The monorepo uses **workspace protocols** for internal dependencies
- All packages must be built before running the full application
- E2E tests require the application to be running
- Node development follows specific patterns for credentials and operations
- The frontend uses a sophisticated canvas system for workflow editing