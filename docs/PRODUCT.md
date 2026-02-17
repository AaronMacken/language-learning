# Product — MVP Scope & UX

## MVP scope

- **One language:** Spanish only.
- **8 verbs**, 2 screens per verb: "I need to [verb]" (Necesito + infinitive) and "I [verb]" (conjugated -o form).
- Single-habit sentence builder that teaches tense and structure through variation. No auth, no AI in the fake MVP.

## UX flow (first 5–10 minutes)

1. **Landing** — No signup. One sentence: "Learn a language by narrating your real life." Button: "Start with something you do every day."
2. **Habit picker** — 8 big buttons: Eat, Work, Study, Use, Buy, Prepare, Drink, Cook. User picks one.
3. **Screen 1** — "I need to [verb]" / "Necesito [infinitive]" (e.g. Necesito comer). Optional: highlight verb, show translation, tiny explanation without grammar jargon.
4. **Screen 2** — "I [verb]" / "[conjugated]" (e.g. Como).
5. **Branch** — (Later) Past, Future, Question. User taps; you explain difference, not rules.
6. **Soft commitment (optional)** — After 5–10 minutes: "Want to keep narrating your day tomorrow?" Email optional. No pressure.

## Validation goal

MVP is successful if:

- A user completes at least one full verb flow.
- User expresses understanding of the pattern.
- UX feels simple and obvious.
- Implementation remains under-engineered and easy to modify.

## Content table (8 verbs)

| Verb (EN) | Screen 1 (I need to \_\_\_) | Screen 2 (I \_\_\_) |
| --------- | --------------------------- | ------------------- |
| eat       | Necesito comer              | Como                |
| work      | Necesito trabajar           | Trabajo             |
| study     | Necesito estudiar           | Estudio             |
| use       | Necesito usar               | Uso                 |
| buy       | Necesito comprar            | Compro              |
| prepare   | Necesito preparar           | Preparo             |
| drink     | Necesito beber              | Bebo                |
| cook      | Necesito cocinar            | Cocino              |

**Pattern:** Necesito = "I need". When you do an action, Spanish often ends the verb with -o. Verbs are first shown in their simplest, most regular form. No reflexive verbs, no irregulars, no tense explanations initially.

## Design principles

- **Pattern-first** — One pattern repeated many times. Show how one sentence mutates; never show lists of unrelated phrases.
- **No grammar jargon** — No "conjugation," "infinitive," etc. Explain meaning and difference, not rules.
- **Confidence > coverage** — User should feel: "I already know how to say things." Progress = depth, not breadth.
- **Always start from something they already know** — This should feel like thinking out loud, not studying. If users say "It feels obvious in a good way," you're winning.
- If a verb introduces extra complexity (e.g. _me levanto_), postpone it.

## Technical constraints (MVP)

- Frontend-only.
- No backend.
- No auth.
- No analytics.
- No email integration (soft commitment is UI only).
- No global state library required unless complexity demands it.

## Explicit exclusions (non-negotiable)

Do not build (for MVP):

- Social features
- Spaced repetition systems
- Dictionaries
- Grammar terminology overload
- Multiple difficulty levels
- Offline mode
- Mobile apps (start web)
- Multiple languages
- Speech recognition
- AI conversations
- Streaks, XP, leaderboards
- Grammar trees
- User-generated content

These are optimization, not validation.
