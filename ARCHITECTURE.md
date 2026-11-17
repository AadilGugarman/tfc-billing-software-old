# Application Architecture

This document describes the architecture of the current React + TypeScript + Tauri ERP app.

## Overview

The app is built as a desktop-first ERP system with a modern web frontend and a secure local backend.

### Main layers

1. **Frontend UI**
   - React + TypeScript
   - Vite build system
   - Tailwind CSS styling
   - React Router for navigation
   - Zustand for client-side state management

2. **Backend / Native layer**
   - Tauri as the desktop shell
   - Rust backend for secure local execution
   - SQLite database accessed through `rusqlite`
   - JWT auth, encryption, and file system operations in Rust

3. **Integration layer**
   - `@tauri-apps/api` for frontend-to-backend communication
   - Tauri commands expose backend operations to the UI
   - Services/hooks translate UI actions into backend requests

## Component Structure

### Frontend directories

- `src/pages/` — route-backed page components
- `src/components/` — reusable UI components and widgets
- `src/components/settings/` — settings sections and page components
- `src/hooks/` — reusable hooks, auth management, keyboard handling, pagination, focus control
- `src/services/` — auth, backup, and other service abstractions
- `src/stores/` — global state management with Zustand
- `src/db/` — database helpers, schema, and data access utilities
- `src/router/` — routing and protected-route logic
- `src/utils/` — small utilities like token registry and helpers

### Backend directories

- `src-tauri/src/` — Rust backend source
- `src-tauri/Cargo.toml` — backend dependency definitions and feature configuration
- `src-tauri/tauri.conf.json` — Tauri configuration for desktop packaging

## Data Flow

### Normal request flow

1. User interacts with the UI in React.
2. UI component calls a service or hook.
3. Service invokes a Tauri command via `@tauri-apps/api`.
4. Rust backend command runs logic, reads/writes SQLite, and returns data.
5. Frontend receives result, updates state, and re-renders.

### Example: loading company data

- `AppRoutes` loads page components lazily.
- Page component uses `usePageData()` or store selectors.
- `usePageData()` triggers Tauri-backed queries if data is missing.
- Backend returns company/party/invoice data from SQLite.

## Authentication Architecture

### Token model

- **Access token**
  - JWT with 15-minute expiry
  - Stored in memory on the frontend
  - Used for verifying user identity on protected backend commands

- **Refresh token**
  - JWT with 30-day expiry
  - Stored securely via `localStorage`
  - Rotated on each refresh and revoked at logout

### Auth commands

- `login` — verify credentials and return access + refresh tokens
- `refresh_access_token` — rotate refresh token and return new tokens
- `verify_access_token` — validate access token and return user info
- `logout` — revoke refresh token server-side
- `setup_initial_admin` — initial admin creation flow

## Routing and Lazy Loading

- `src/router/routes.tsx` uses `React.lazy()` and `<Suspense>`.
- Pages are loaded on demand, reducing initial bundle weight.
- `vite.config.ts` includes manual chunking and separate vendor chunk rules.
- `vite-plugin-singlefile` is used for single-file production packaging, which may hide chunk files in the shipped desktop app.

## Performance Considerations

- Heavy libraries like `ag-grid`, `recharts`, and `framer-motion` are excluded from dependency pre-bundling.
- Common modules are pre-bundled for faster dev startup.
- Code-splitting is handled at the route/page level.

## Security Considerations

- JWT secret is stored outside source control in app data.
- Refresh tokens are hashed in the database before storage.
- Passwords are hashed with Argon2, with bcrypt legacy support and migration.
- Rust backend enforces token type checks and user active status.

## Recommended Architecture Improvements

For a more resilient future architecture, consider adding:

- explicit backend API layer abstraction for better separation of concerns
- typed request/response contracts between frontend and Tauri commands
- a small service bus or gateway for backend requests
- documentation of domain entities and data model relationships
- architecture decision records (ADRs) for major technical choices

## Summary

This app architecture is a solid hybrid model: modern React frontend, secure local Rust backend, and desktop packaging via Tauri. It is well-suited for a self-contained desktop ERP application.
