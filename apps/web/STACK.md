# Web App Stack — apps/web

## Purpose

Implements the 5–10 minute MVP learning flow described in:

- docs/PRODUCT.md
- ARCHITECTURE.md

This app is frontend-only for MVP.

---

## Core Stack

- React
- TypeScript (strict)
- Vite
- Tailwind CSS
- React Router (simple routing only)

---

## Routing Model (MVP)

- Use React Router for basic screen transitions.
- No nested routing complexity.
- No route guards.
- No URL query-state management required.
- Linear flow:
  - Landing
  - Habit picker
  - Verb Screen 1
  - Verb Screen 2
  - Optional soft commitment

Keep routes simple and predictable.

---

## State Management

- Use local React state only.
- No global state libraries (Redux, Zustand, etc.).
- No React Query.
- No context unless prop drilling becomes painful.
- No persistence required.

All verb data is hardcoded.

---

## Data Model (MVP)

Verb data should follow this shape:

{
id: string
english: string
infinitive: string
presentYo: string
}

Data lives locally inside the web app for MVP.
No APIs.
No backend calls.

---

## Styling Rules

- Tailwind only.
- No design systems.
- No component libraries.
- Avoid excessive utility stacking — prioritize readability.
- Mobile-first layout.
- Clean typography > visual complexity.

Keep UI simple and calm.

---

## Folder Structure

Inside apps/web:

- src/
  - screens/ # high-level flow screens
  - components/ # reusable UI components
  - data/ # hardcoded verb data
  - routes/ # router config (minimal)
  - hooks/ # local hooks (if needed)

Keep structure flat and obvious.
Do not over-nest.

---

## Component Guidelines

- Screens = route-level components.
- Components = reusable UI building blocks.
- Keep components small and explicit.
- Do not abstract prematurely.
- Avoid "generic pattern engines" for MVP.

---

## Performance

- No premature optimization.
- No memoization unless necessary.
- Keep logic simple and readable.
