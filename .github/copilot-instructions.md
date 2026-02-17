# Copilot Instructions (Always-On)

## Product North Star

- App: Learn a foreign language by learning words around things you habitually do on a daily basis
- Target user: People who want a practical, habitual, systematic approach to learning foreign language (spanish only for now)
- MVP success: First 5-10 minutes of UX that take users through the structure of learning language around their daily habits
- (Reference when more information is needed on UX) **[docs/PRODUCT.md](docs/PRODUCT.md)** — MVP scope, UX flow, content spec, design principles, exclusions

## Non-Negotiables

- Do not invent APIs, endpoints, data fields, or env vars. If unclear, ask.
- Prefer smallest safe change; avoid refactors unless requested.
- Handle loading/error/empty states for user-facing data.
- Follow accessibility basics: labels, keyboard nav, focus, aria where needed.
- Never log secrets/tokens/PII.

## Tech + Repo Conventions (Monorepo)

- Repo: monorepo. Prefer package-local conventions over global assumptions.
- Layout:
  - apps/\* = runnable applications
  - packages/\* = shared libraries (domain, utils, config)
- Boundaries:
  - apps may depend on packages
  - packages must not import from apps
  - avoid circular dependencies
- TypeScript: strict; avoid `any`; prefer explicit types at boundaries.
- Imports: prefer path aliases defined per package; avoid deep relative imports across package boundaries.
- Formatting/Lint/Tests: use the repo scripts; do not add new tooling without matching existing patterns.

## Domain Awareness

- Keep language-learning logic inside domain packages, not UI components.
- Separate presentation (UI) from learning logic (spaced repetition, verb patterns, content structure).
- Favor explicit domain models over loosely typed objects.

## Decision Hierarchy

When making decisions, prioritize in this order:

1. Product clarity and MVP simplicity
2. Maintainability and clear domain modeling
3. Type safety and correctness
4. Performance (optimize only when needed)
5. Abstraction reuse (avoid premature generalization)

## How to Respond / Deliver

- If requirements are ambiguous, ask 1–3 clarifying questions, then propose defaults.
- Provide code in minimal diffs and include filenames.
- When touching logic, add/update tests where practical.
- Briefly state assumptions and tradeoffs when relevant.
