
I want to execute the plan one phase at a time. When I tell you which phase (and optional step) I'm on, walk me through the exact commands and file changes, and help me verify the step before moving on.

**Phase 1 — Repo and web app scaffold:** Root package manager (pnpm preferred), create apps/web with Vite (react-ts template), add Tailwind to apps/web. Optional: pnpm workspaces. Done when: pnpm install works, dev server runs, Tailwind applies.

**Phase 2 — Linting and formatting:** ESLint (flat config, TypeScript + React + React-Hooks) and Prettier in apps/web, eslint-config-prettier so they don't conflict. Scripts: lint, format. Done when: lint and format run clean.

**Phase 3 — Testing:** Vitest + React Testing Library + jsdom in apps/web (setup file, vite.config test config); Playwright for E2E. Scripts: test, test:run, test:e2e. Done when: test:run and test:e2e pass.

**Phase 4 — GitHub Actions CI:** .github/workflows/ci.yml on push/PR to main: install (Node 20, pnpm), lint, format check, unit tests, E2E (Playwright). Done when: PR triggers workflow and jobs pass.

**Phase 5 — Git hygiene:** Husky at root, lint-staged (ESLint + Prettier on staged files in apps/web), commitlint (conventional commits), pre-commit and commit-msg hooks. Done when: bad commit message is rejected and lint-staged runs on commit.

I'm currently on: [e.g. "Phase 1" or "Phase 2, step 2"]. Guide me through it step-by-step.