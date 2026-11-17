# Developer Guide for React + Tauri ERP Apps

This guide is tailored for the current app architecture: a React + TypeScript frontend packaged with Tauri, using SQLite via Rust and Drizzle ORM.

## 1. Core Technology Recommendations

- Keep React and TypeScript as the UI foundation.
- Use Vite for both development and production builds.
- Use Tailwind CSS for fast, utility-first styling.
- Use Tauri for desktop packaging when you need a lightweight native shell.
- Use SQLite for local desktop data with a Rust-backed persistence layer.
- Prefer Rust backend logic for secure local processing, authentication, and file system access.

## 2. Code Quality and Tooling

### Essential tools

- ESLint with `eslint-plugin-react`, `@typescript-eslint/eslint-plugin`, and `eslint-plugin-react-hooks`
- Prettier for formatting consistency
- TypeScript strict mode enabled in `tsconfig.json`
- `simple-import-sort` or `eslint-plugin-import` for clean import order

### Why these matter

- Ensures consistent code style across frontend and backend.
- Improves readability and reduces bugs early.
- Makes onboarding faster for new developers.

## 3. Testing Strategy

### Recommended test types

- Unit tests for pure React components and utility functions
- Component tests for UI behavior with React Testing Library
- Integration tests for auth flows, form validation, and state management
- End-to-end tests for critical user journeys like login, company creation, invoice preview, and backup/restore

### Suggested tools

- `vitest` for unit and component tests
- `@testing-library/react` for component assertions
- `@testing-library/jest-dom` for DOM expectations
- `playwright` for end-to-end desktop/web scenario testing

### Example test targets

- `src/services/auth.ts` token lifecycle and refresh logic
- `src/components/company-wizard/*` stepper validation and navigation
- `src/router/routes.tsx` access control and route guards
- `src/components/settings/*` save/update behavior
- `src/shops` and store logic for data loading and persistence

## 4. Performance and Optimization

### Current good practices in this app

- Route-based lazy loading with `React.lazy()` and `Suspense`
- Vendor chunking in `vite.config.ts`
- Pre-bundling frequent modules via `optimizeDeps`
- Excluding heavy libraries from dependency pre-bundling
- Single-file packaging for desktop release via `vite-plugin-singlefile`

### Recommended additions

- Add `rollup-plugin-visualizer` or `@bundle-buddy/rollup-plugin` to inspect bundle size.
- Use dynamic imports for heavy components and libraries such as AG Grid, Recharts, and Framer Motion when not needed immediately.
- Prefetch the next likely route after login or on hover.
- Lazy-load large modals and preview components if they are not required at startup.
- Enable compression for web builds (gzip/ Brotli) if you ever deploy a web variant.

## 5. Authentication and Security

### Best practices for this stack

- Keep access tokens in memory and refresh tokens in secure storage.
- Rotate refresh tokens on every refresh.
- Revoke refresh tokens on logout.
- Validate JWT type (`access` vs `refresh`) explicitly.
- Store the JWT secret outside source control in the app data folder.

### Desktop-specific notes

- Avoid keeping sensitive tokens in files unless they are encrypted.
- Do not rely on URL-based auth flows for desktop apps.
- Use Tauri commands to mediate all authentication operations instead of direct browser storage access.

## 6. Architecture and Code Organization

### Recommended app structure

- `src/pages/` for route-backed pages
- `src/components/` for reusable UI pieces
- `src/components/settings/` for settings sections
- `src/hooks/` for reusable hooks and app behaviors
- `src/services/` for API/auth/data access logic
- `src/stores/` for application state and persisted settings
- `src/db/` for data access and schema definitions
- `src/router/` for route definitions and protected routes

### Why this helps

- Clean separation reduces coupling and improves maintainability.
- Easier to locate logic when extending the app.
- Makes it simpler to implement code splitting and lazy loading.

## 7. Release and Build Practices

### Desktop release flow

- Use `npm run desktop:build` for Tauri packaging.
- Validate the generated app bundles and test installation.
- Keep `vite-plugin-singlefile` enabled only for the desktop production target.

### Web build flow (if you add web deploy)

- Use `npm run build`.
- Review generated assets and chunk sizes.
- Use compression and caching headers in hosting.

## 8. Future Enhancements and Scaling

### Add gradually when needed

- CI/CD: GitHub Actions for lint, tests, build, and desktop packaging.
- Monorepo support if the app grows into multiple packages.
- Analytics and error reporting for production desktop installs.
- Feature flags for gradual rollouts.
- Multi-tenant or multi-company separation if the product grows beyond single-instance desktop usage.

### Persistent reference points

- Keep `TECH_STACK.md` updated with actual dependency versions.
- Keep this guide updated if architecture changes.
- Add a `CONTRIBUTING.md` later for team conventions and PR expectations.

## 9. Recommended file additions

- `TECH_STACK.md` — current stack and versions
- `DEVELOPER_GUIDE.md` — this document
- `CONTRIBUTING.md` — workflow, branch naming, commit style
- `CODE_OF_CONDUCT.md` — team collaboration guidelines
- `CHANGELOG.md` — release history and fixed issues

## 10. World-class practices to add

### Onboarding and setup

- Add `README.md` with project purpose, local setup, and build/run commands.
- Add an `environment` section for required Node, Rust, and Tauri versions.
- Document the database setup, migration workflow, and local dev database reset process.

### Documentation and knowledge sharing

- Maintain architecture decision records (ADRs) for major technology choices.
- Document key domain models, data flows, and auth/security policies.
- Keep a `FEATURES.md` or `ARCHITECTURE.md` for high-level system diagrams.

### Accessibility and internationalization

- Add accessibility checks and keyboard navigation guidance.
- Define aria/semantic markup practices for UI components.
- Document localization process and how to add new languages.

### Observability and error handling

- Recommend logging and error capture for both frontend and backend.
- Add a section for handling user-facing errors and crash recovery.
- Document any monitoring or analytics approach if added later.

### Quality and process

- Define code review expectations, PR checklist, and testing coverage goals.
- Add a branching strategy, release cadence, and versioning policy.
- Document security review and dependency upgrade cadence.

### Performance and maintainability

- Add performance budgets for bundle size, initial load, and runtime interactions.
- Document when to create an optimization spike versus a permanent refactor.
- Track technical debt and code cleanup tasks explicitly.

## 11. Closing note

This document is a strong foundation, but world-class engineering is not just about stack and tooling — it also requires process, documentation, accessibility, observability, and continuous improvement.

Use this guide as the starting point, then expand it with concrete team practices and governance for the next level of quality.

---

This document is intended as a future reference for this app type. Use it when onboarding new developers, planning new features, or preparing releases.
