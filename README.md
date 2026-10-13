# SummerNote

A self-guided learning project: build a Django web app for daily notes with an AI day-summarizer, by following the phase-by-phase tutorial in [`docs/`](docs/).

**You** type every command. **The docs** tell you what to type, what should happen, how to tell if it worked, and *why* each step matters. The repo doubles as your working code and your learning journal — Git history records your progress.

## What you'll build

A Django web app where a logged-in user writes dated notes in a notepad UI, sees them laid out on a monthly calendar and a 24-hour day view, and asks an AI model to summarize any day's notes.

## How to use this repo

1. Clone it.
2. Open [`docs/README.md`](docs/README.md) — the tutorial index.
3. Start at `docs/phases/00-prereqs.md`.
4. Work through phases in order. Each phase file has: goal, concepts, commands to run, expected output, validation, reflection prompts.
5. Commit your own work to this repo as you go. Branch per feature (`feature/<step>-<slug>`), open a PR, self-review, merge.

## Companion docs

- [`docs/project-roadmap.md`](docs/project-roadmap.md) — all phases at a glance; what's done, what's next.
- [`docs/phases/`](docs/phases/) — one file per step.
- [`docs/templates/phase.md`](docs/templates/phase.md) — the template every phase file follows (so you know what to expect).

## Learning stance

- The docs assume you'll stop, read, type, and verify at each step — not scan ahead.
- When something breaks, **read the error, then search the docs' troubleshooting section before asking an LLM**. The debugging muscle is the point.
- Every phase has reflection prompts. Write your answers in your own notes (or in a `docs/reflections/` folder you add). Explaining in your own words is where the learning crystallizes.
