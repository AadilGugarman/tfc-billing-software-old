# Tech Stack Overview

## Frontend

- React 19.2.6
- TypeScript 5.9.3
- Vite 7.3.2
- Tailwind CSS 4.1.17
- React Router DOM 7.15.0
- Zustand 5.0.13
- Framer Motion 12.38.0
- Recharts 3.8.1
- AG Grid (`ag-grid-community`, `ag-grid-react`) 35.2.1
- i18next 26.1.0 + react-i18next
- Lucide React icons
- Sonner toast notifications
- date-fns for date handling
- clsx for class merging
- tailwind-merge for Tailwind class deduplication

## Data / ORM

- Drizzle ORM 0.45.2
- Drizzle Kit 0.18.1
- SQLite via Rust `rusqlite`

## Desktop / Backend

- Tauri 2.6.1
- Rust 2021 edition
- `@tauri-apps/api` 2.11.0
- JWT auth with `jsonwebtoken` 9
- Password hashing with Argon2 0.5
- Legacy bcrypt support 0.15
- sqlite access with `rusqlite`
- `serde` + `serde_json`
- `chrono` for time handling
- `dirs` for OS-specific app paths
- `zip` for backup/export functionality
- `uuid` for IDs
- `rand`, `rand_core`, `hmac`, `sha2`, `password-hash`

## Build / Tooling

- Vite build pipeline
- `@vitejs/plugin-react`
- `@tailwindcss/vite`
- `vite-plugin-singlefile` for production desktop packaging
- Terser minification
- TypeScript compiler

## Authentication and Security

- JWT-based access tokens (15-minute expiry)
- JWT refresh tokens (30-day expiry)
- Refresh token rotation + server-side revocation
- In-memory access token storage in frontend
- Refresh token persistence in `localStorage`
- Rust backend verification and authorization guards

## Why this stack is enough and industry practice

Yes — this is a strong, modern stack for a desktop-first ERP app built with React + Tauri.

- React + TypeScript + Vite is a widely accepted industry standard for modern UI development.
- Tailwind CSS is common for productivity and rapid UI styling.
- Tauri is a valid choice for lightweight desktop apps using web frontend code.
- Drizzle ORM is a good fit for TypeScript database access and works well with SQLite.
- JWT + refresh tokens is a standard auth approach for single-page apps and desktop apps.

### When this stack is enough

This stack is enough when your main priorities are:

- fast UI development
- type safety
- native desktop packaging
- local data storage with SQLite
- clear separation of frontend and backend logic

### Additional industry-standard tooling you may add later

These are useful, but not required to call the stack "industry practice":

- ESLint + Prettier for consistent code style
- Vitest / React Testing Library for unit/component testing
- Playwright for end-to-end tests
- CI pipelines (GitHub Actions, GitLab CI, etc.)
- bundle analysis tooling
- lint-staged / husky for pre-commit checks
- API docs or typed SDK generation

## Lazy loading in this app

Lazy loading is already present in the router configuration.

- `src/router/routes.tsx` uses `React.lazy()` for page imports.
- `AppRoutes` renders these inside `<Suspense>`.
- This means page code is loaded on demand, not all at once.

### Why you may not see it immediately

- The entry file `src/App.tsx` only renders `AppRoutes`; the lazy-loading logic lives in `src/router/routes.tsx`.
- Production desktop packaging uses `vite-plugin-singlefile`, which bundles assets into one file for Tauri. That can hide separate chunk files in the final package even if the code path is set up for lazy loading.
- In development or non-singlefile builds, you should see route chunks created by Vite when the app is built.

## Notes

- The current stack is solid for your application.
- You can enhance it later with testing, linting, CI, and bundle analysis without changing the core technology choices.
- If you want, I can also add a `DEV_STACK.md` section listing recommended tooling and best practices for future scaling.
