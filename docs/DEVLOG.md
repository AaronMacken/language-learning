# Development Log

## Phase 1: Monorepo & Web App Scaffold

### Architecture Decisions

#### Why Monorepo?

**Decision:** Use a monorepo structure with pnpm workspaces.

**Rationale:**

- **Code Sharing:** Share TypeScript types, domain logic, UI components, and utilities across web, mobile, and API without publishing packages
- **Atomic Changes:** Update shared code and all consuming apps in a single PR; test together, deploy confidently
- **Consistency:** Single source of truth for ESLint, TypeScript, Prettier, and dependency versions
- **No Version Hell:** Eliminate version mismatch issues and multi-repo dependency update overhead
- **Future Mobile App:** Planned React Native app can reuse domain logic and shared utilities

**Tradeoffs:**

- Larger repo size (mitigated by pnpm's content-addressable storage)
- Need discipline to maintain package boundaries (apps can depend on packages, but packages cannot depend on apps)
- All developers need pnpm installed

**Boundary Rules:**

```
apps/*     → can import from packages/*
packages/* → cannot import from apps/*
packages/* → avoid circular dependencies between packages
```

---

#### Why pnpm?

**Decision:** Use pnpm instead of npm or yarn.

**Rationale:**

- **Workspace Support:** First-class monorepo support with efficient symlink-based local package resolution
- **Disk Efficiency:** Content-addressable global store means packages are stored once and hard-linked into node_modules
- **Strict by Default:** Enforces that dependencies must be declared (prevents phantom dependencies)
- **Fast:** Faster installs than npm/yarn in most cases

**Setup (requires Node v16.9+):**

```bash
corepack enable
corepack prepare pnpm@latest --activate
```

**How It Works:**

- pnpm-workspace.yaml declares `apps/*` and `packages/*` as workspace packages
- Each folder with a package.json becomes a local package
- On `pnpm install`, pnpm creates symlinks in node_modules pointing to local packages
- Importing `import { X } from 'domain'` resolves to the live `packages/domain` folder via symlink

**Example:**

```typescript
// In apps/web/src/App.tsx
import { myUtil } from 'domain'; // Resolves to packages/domain via symlink
```

---

#### Why Vite (vs Next.js, CRA, or Webpack)?

**Decision:** Use Vite for the web app build tool.

**Rationale:**

- **Dev Speed:** Native ESM in dev = no bundling upfront; esbuild transforms TypeScript/JSX instantly
- **Production-Ready:** Uses Rollup for optimized production builds with tree-shaking
- **Simplicity:** No framework lock-in (unlike Next.js); explicit config (unlike CRA)
- **Flexibility:** Can add React Router for routing, Tailwind for styling—bring your own stack
- **Better for SPA MVP:** This app doesn't need SSR or file-based routing yet

**Tradeoffs:**

- No SSR out of the box (Next.js would provide this, but MVP doesn't need it)
- Need to configure routing, data fetching, auth manually (vs Next.js conventions)
- Less opinionated = more decisions to make

**When to Reconsider:**

- If SEO becomes critical → migrate to Next.js with SSR
- If we add a public marketing site → add a separate Next.js app to the monorepo

**Vite vs Webpack vs Rollup:**

| Tool    | Purpose          | Dev Experience           | Production          |
| ------- | ---------------- | ------------------------ | ------------------- |
| Webpack | Bundler          | Slower (bundles upfront) | Highly configurable |
| Rollup  | Bundler          | Not designed for dev     | Best tree-shaking   |
| Vite    | Dev + Build Tool | Instant (native ESM)     | Uses Rollup         |

**Key Insight:** Vite is a **dev server** (serves native ES modules) + **build tool** (uses Rollup for production). It does not bundle during dev, making HMR nearly instant.

---

### Repository Structure

```
language-learning/
├── apps/
│   ├── web/          # Vite + React + TypeScript SPA
│   ├── mobile/       # (planned) React Native
│   └── api/          # (planned) Backend API
├── packages/
│   ├── domain/       # Core learning logic (spaced repetition, content models)
│   ├── utils/        # Shared utilities
│   └── config/       # Shared config (ESLint, TS, etc.)
├── docs/
│   ├── PRODUCT.md
│   ├── ARCHITECTURE.md
│   └── DEVLOG.md
├── package.json
├── pnpm-workspace.yaml
└── pnpm-lock.yaml
```

**Root package.json:**

```json
{
  "name": "language-learning",
  "private": true,
  "scripts": {
    "dev:web": "pnpm --filter ./apps/web dev",
    "build:web": "pnpm --filter ./apps/web build"
  }
}
```

- `"private": true` prevents accidental publishing to npm
- `--filter` targets specific workspace packages

---

### Configuration Files (apps/web)

#### TypeScript: Dual Runtime Setup

**Files:**

- `tsconfig.json` (references app and node configs)
- `tsconfig.app.json` (browser runtime: React app code)
- `tsconfig.node.json` (Node runtime: vite.config.ts)

**Why Two Runtimes?**

- Vite runs in Node.js but bundles code for the browser
- vite.config.ts needs Node types and module resolution
- App code needs DOM types and browser globals

**Example:**

```typescript
// vite.config.ts → uses tsconfig.node.json (Node typings, no DOM)
// src/App.tsx → uses tsconfig.app.json (DOM typings, browser globals)
```

---

#### ESLint Config (eslint.config.js)

**Extends:**

- `@eslint/js` – Core JavaScript rules
- `typescript-eslint` – TypeScript-specific linting
- `eslint-plugin-react-hooks` – Enforces Rules of Hooks
- `eslint-plugin-react-refresh` – Ensures HMR compatibility (Vite-specific)

**Globals:**

- `globals.browser` – Tells ESLint code runs in browser (window, document, etc.)

**Why Not Airbnb?**

- Airbnb config is opinionated and sometimes conflicts with modern TypeScript patterns
- Starting minimal, will add custom rules as needed

---

#### Vite Config (vite.config.ts)

**Current Setup:**

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()], // JSX transform + Fast Refresh
});
```

**Common Future Additions:**

**Path Aliases:**

```typescript
resolve: {
  alias: {
    '@': '/src',
    'domain': '../packages/domain/src'
  }
}
```

**API Proxy (for local dev):**

```typescript
server: {
  proxy: {
    '/api': 'http://localhost:3000'
  }
}
```

**Code Splitting / Chunking** can be configured in `build.rollupOptions`.

---

### Scaffold Command

```bash
pnpm create vite apps/web --template react-ts
```

**What This Does:**

- Scaffolds React + TypeScript project in apps/web
- Generates vite.config.ts, tsconfig.json, ESLint config
- Similar to Create React App but does not hide build config
- Provides full control over Vite configuration

---

## Key Takeaways

### Monorepo Benefits

- Share code without publishing
- Atomic cross-package updates
- Single config strategy

### pnpm Benefits

- Efficient storage (global content-addressable store)
- Symlink-based workspace resolution
- Strict dependency enforcement

### Vite Benefits

- Instant dev server (native ESM)
- Production builds via Rollup
- Explicit, minimal config
- No SSR overhead for SPA MVP

### Tradeoffs Made

- No SSR (can add Next.js later if needed)
- Manual routing/state setup (bring your own stack)
- Monorepo discipline required (enforce package boundaries)

---

## Potential Evolution Paths

**At 1k Users:**

- Add caching layer (React Query, SWR)
- Optimize bundle size (lazy loading, code splitting)

**At 10k Users:**

- CDN for static assets
- Consider API rate limiting
- Monitor bundle size / Core Web Vitals

**Future Architecture Changes:**

- Add React Native mobile app (reuse domain package)
- Add backend API (Node/Express or similar in apps/api)
- If SEO needed → add Next.js for public pages
- If global state grows → add Zustand or Jotai (prefer local state first)
