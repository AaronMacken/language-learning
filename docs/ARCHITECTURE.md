# Architecture

## Goals

- Keep all apps centralized while allowing independent deployments.
- Share core business logic and types across web, mobile, and backend.
- Avoid premature microservices while keeping a clear path to scale.
- Make the architecture easy to explain in system-design discussions.

## Repository structure

The monorepo is organized into:

```
apps/
  web/      # React + TypeScript web app (MVP)
  mobile/   # Mobile app (tech TBD)
  api/      # Backend API (tech TBD)

packages/
  domain/   # Core business logic & domain models (learning progress, lessons, spaced repetition rules, etc.)
  utils/    # Generic helper functions (no business meaning)
  ui/       # Shared, framework-agnostic UI components (optional)
  config/   # Shared tooling (tsconfig, eslint, etc.)
```

## Build system

- **Turborepo** manages the monorepo, providing:
  - Intelligent task execution and caching across apps and packages.
  - Automatic dependency graph detection to only build affected workspaces.
  - Parallel builds to optimize CI/CD performance.
  - Consistent scripts across all workspaces via `turbo.json`.

## Domain layer rules

- Contains product-defining logic (not framework-specific).
- No UI, no database, no network code.
- Safe to import from any app.
- Changes here may affect multiple apps.

## Tech stack (MVP)

- **Frontend:** React + TypeScript, Vite (or Next.js). Minimal styling (Tailwind or plain CSS). Mobile-first, web only for MVP.
- **Backend (later):** Option A — Supabase (auth, Postgres, RLS, server actions or API routes). Option B — No auth at first; local storage + anonymous sessions; add backend when users ask for accounts.

## Data model (simple, for future backend)

- **habits** — `id`, `label_en` (e.g. "brush my teeth").
- **sentence_variations** — `habit_id`, `tense` (present/past/future), `form` (statement/question/negative), `target_sentence`, `breakdown` (JSON chunks).

## Deployment model

- Each app (web, mobile, api) is built and deployed independently.
- CI/CD uses path-based detection to deploy only affected apps.
- Shared packages do not deploy on their own.

## Constraints

- Prefer simplicity and clarity over clever abstractions.
- Avoid microservices until there is a clear scaling reason.
- Favor explicit boundaries that can be explained in interviews.
- Assume modern TypeScript tooling; fit this structure without over-engineering.
