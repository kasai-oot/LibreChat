# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

LibreChat is an open-source AI chat platform with multi-model support (OpenAI, Anthropic, Google, Azure, AWS Bedrock, etc.). It features a full-stack architecture with Express.js backend, React frontend, and MongoDB database.

## Commands

### Development Setup

**Initial Setup:**
```bash
# Install dependencies (Node.js 20.x required)
npm ci

# Install TypeScript globally
npm i -g typescript

# Build packages in order (required before development)
npm run build:data-provider
npm run build:data-schemas
npm run build:api
```

**Update/Reinstall:**
```bash
# Update from main branch
npm run update

# Reinstall all packages
npm run reinstall

# For Docker environment
npm run reinstall:docker
```

### Running the Application

**Local Development:**
```bash
# Start backend (development mode with nodemon)
npm run backend:dev

# Start frontend (development mode with hot reload)
npm run frontend:dev

# Build frontend for production
npm run frontend
```

**Production:**
```bash
# Start backend in production mode
npm run backend

# Build client for production
cd client && npm run build
```

**Docker:**
```bash
# Start deployed containers
npm run start:deployed

# Stop deployed containers
npm run stop:deployed

# Update deployed version
npm run update:deployed
```

**Bun (alternative runtime):**
```bash
# Backend with Bun
npm run b:api

# Frontend with Bun
npm run b:client:dev
```

### Testing

**Unit Tests:**
```bash
# Backend tests
npm run test:api

# Frontend tests
npm run test:client

# Watch mode (client)
cd client && npm run test
```

**Integration Tests (E2E with Playwright):**
```bash
# Run e2e tests locally
npm run e2e

# Run with UI visible
npm run e2e:headed

# Debug mode
npm run e2e:debug

# Generate test code
npm run e2e:codegen

# View test report
npm run e2e:report
```

### Code Quality

**Linting & Formatting:**
```bash
# Run ESLint
npm run lint

# Auto-fix linting issues
npm run lint:fix

# Format code with Prettier
npm run format

# Type checking (client)
cd client && npm run typecheck
```

### Database & User Management

**User Administration:**
```bash
# Create new user
npm run create-user

# Invite user (send invitation)
npm run invite-user

# List all users
npm run list-users

# Reset user password
npm run reset-password

# Ban user
npm run ban-user

# Delete user
npm run delete-user
```

**Balance Management:**
```bash
# Add balance to user account
npm run add-balance

# Set balance for user account
npm run set-balance

# List user balances
npm run list-balances

# Get user statistics
npm run user-stats
```

**Database Operations:**
```bash
# Reset Meilisearch sync
npm run reset-meili-sync

# Flush cache
npm run flush-cache

# Reset terms acceptance
npm run reset-terms
```

**Migrations:**
```bash
# Agent permissions migration (dry-run)
npm run migrate:agent-permissions:dry-run

# Run agent permissions migration
npm run migrate:agent-permissions

# Prompt permissions migration
npm run migrate:prompt-permissions
```

### Banner Management

```bash
# Update banner
npm run update-banner

# Delete banner
npm run delete-banner
```

## Architecture

### Monorepo Structure

LibreChat uses **npm workspaces** with the following structure:

- **`api/`** - Backend Express.js server
- **`client/`** - React frontend application
- **`packages/`** - Shared packages:
  - **`data-provider/`** - Data services layer (API clients, data fetching)
  - **`data-schemas/`** - Mongoose schemas and validation (Zod schemas)
  - **`api/`** - Shared API utilities and types
  - **`client/`** - Shared client utilities

**Build Order Dependency:** Packages must be built before the main application:
1. `data-schemas` (defines data structures)
2. `data-provider` (uses schemas)
3. `api` package (uses both)
4. Client/API apps (consume all packages)

### Backend Architecture

**Entry Point:** `api/server/index.js`

**Core Structure:**
- **`api/server/controllers/`** - Request handlers
- **`api/server/routes/`** - Express route definitions
- **`api/server/services/`** - Business logic layer
- **`api/server/middleware/`** - Express middleware (auth, validation, rate limiting)
- **`api/models/`** - Mongoose models and database methods

**Authentication:**
- Multi-strategy authentication: JWT, LDAP, OAuth2 (Google, GitHub, Discord, Facebook, Apple, SAML)
- Passport.js for authentication strategies
- Express sessions with Redis or memory store

**Key Routes:**
- `/api/auth` - Authentication endpoints
- `/api/messages` - Chat message handling
- `/api/convos` - Conversation management
- `/api/assistants` - AI assistants/agents
- `/api/files` - File upload/management
- `/api/search` - Conversation search (Meilisearch)
- `/api/config` - Application configuration
- `/api/models` - Available AI models
- `/api/endpoints` - AI provider endpoints
- `/api/mcp` - Model Context Protocol servers

**Database:**
- **MongoDB** - Primary data store (conversations, users, messages)
- **Meilisearch** - Full-text search indexing
- **PostgreSQL with pgvector** - Vector database for RAG API
- **Redis** (optional) - Session store and caching

### Frontend Architecture

**Entry:** `client/src/`

**Core Structure:**
- **`client/src/components/`** - React components (organized by feature)
- **`client/src/routes/`** - React Router pages
- **`client/src/store/`** - State management (Recoil + Jotai)
- **`client/src/hooks/`** - Custom React hooks
- **`client/src/data-provider/`** - API integration layer
- **`client/src/utils/`** - Utility functions

**State Management:**
- **Recoil** - Primary state management
- **Jotai** - Atom-based state for specific features
- **React Query (@tanstack/react-query)** - Server state, caching, data fetching

**UI Libraries:**
- **Radix UI** - Accessible component primitives
- **Tailwind CSS** - Styling
- **Framer Motion** - Animations
- **React Markdown** - Message rendering with syntax highlighting

**Build Tool:** Vite (for fast dev server and optimized production builds)

### Configuration

**Environment Variables:**
- Primary config in `.env` file (see `.env.example`)
- Key variables: `MONGO_URI`, `PORT`, `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, etc.

**YAML Configuration:**
- `librechat.yaml` - Main application config (AI endpoints, interface settings, file storage)
- See `librechat.example.yaml` for all options
- Supports custom endpoints, MCP servers, file strategies, rate limits, UI customization

### File Storage

Configurable strategies (per file type):
- **Local** - Filesystem storage
- **AWS S3** - Cloud object storage
- **Firebase** - Cloud storage with CDN

### AI Integration

**Supported Providers:**
- OpenAI, Azure OpenAI, Anthropic (Claude), Google (Gemini/Vertex AI)
- AWS Bedrock, Custom OpenAI-compatible endpoints
- Local models via Ollama, OpenRouter, Together.ai, etc.

**Features:**
- Multi-modal support (text, images, files)
- Streaming responses (Server-Sent Events)
- Custom agents with tool calling
- Model Context Protocol (MCP) integration
- RAG (Retrieval-Augmented Generation) via separate API
- Code Interpreter (sandboxed code execution)

### Services & Background Jobs

- **Meilisearch Sync** - Background indexing of conversations
- **MCP Initialization** - Model Context Protocol server setup
- **Database Migrations** - Schema updates and data migrations

## Development Workflow

### TypeScript Conversion

- **Frontend:** Nearly complete TypeScript conversion
- **Backend:** Still JavaScript (Node.js), no immediate plans for TypeScript conversion
- Use `.ts`/`.tsx` for frontend, `.js` for backend

### Module Conventions

Imports ordered by:
1. npm packages (longest to shortest)
2. TypeScript types (longest to shortest, package types first)
3. Local imports (longest to shortest, `~` alias same as relative)

ESLint auto-formats imports via `npm run lint:fix`.

### Git Workflow

- Use descriptive branch names: `new/feature/x`, `fix/bug-name`
- Commit format: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`
- Squash commits before PR to maintain clean history

### Testing Requirements

Before submitting PRs:
1. Run `npm run lint` and fix all errors
2. Run `npm run test:api` (backend unit tests)
3. Run `npm run test:client` (frontend unit tests)
4. Clear localStorage/cookies and test manually
5. Run `cd client && npm run build` to check TypeScript compilation
6. For Docker deployments, test with `npm run reinstall:docker`

### Environment Setup for Testing

**Unit Tests:**
- Copy `api/test/.env.test.example` to `api/test/.env.test`

**E2E Tests:**
- Copy `.env.example` to `.env`
- Copy `e2e/config.local.example.ts` to `e2e/config.local.ts`
- Copy `librechat.example.yaml` to `librechat.yaml`
- Install MongoDB locally (`mongosh` should connect)
- Install Playwright: `npx playwright install`

## Key Files & Directories

**Configuration:**
- `.env` - Environment variables
- `librechat.yaml` - App configuration (endpoints, UI, features)
- `docker-compose.yml` - Docker service definitions

**Build Outputs:**
- `client/dist/` - Production frontend build
- `packages/*/dist/` - Compiled package outputs

**Data Directories:**
- `uploads/` - User-uploaded files
- `images/` - Image assets
- `logs/` - Application logs
- `data-node/` - MongoDB data (Docker)
- `meili_data_v1.12/` - Meilisearch index (Docker)

## Important Notes

- Always rebuild packages after pulling changes: `npm run update`
- Restart ESLint server in IDE after package changes
- MongoDB and Meilisearch required for full functionality
- Redis recommended for production (session management)
- Husky pre-commit hooks enforce linting standards
- Use Node.js 20.x (specified in contributing guidelines)
- Frontend hot reload: Changes auto-refresh in dev mode
- Backend nodemon: Auto-restarts on file changes (excludes `data/`, `client/`)
