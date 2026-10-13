# Project Roadmap

What you're building and in what order. Each phase step has its own tutorial file in [`phases/`](phases/).

**Status key:** ⬜ not started · 🟡 in progress · ✅ done · ❌ dropped

---

## Phase 0 — Prerequisites
- [ ] 0.1 Verify Python, Git, `gh` installed and authenticated — [`phases/00-prereqs.md`](phases/00-prereqs.md)

## Phase 1 — Foundation
- [ ] 1.1 Scaffold Django project + `notes` app — [`phases/01-1-scaffold-django.md`](phases/01-1-scaffold-django.md)
- [ ] 1.2 Git init + `.gitignore` + first commit — [`phases/01-2-git-init.md`](phases/01-2-git-init.md)
- [ ] 1.3 Publish to GitHub — [`phases/01-3-github-repo.md`](phases/01-3-github-repo.md)
- [ ] 1.4 Settings split + `.env` + first feature-branch PR — [`phases/01-4-settings-env.md`](phases/01-4-settings-env.md)
- [ ] 1.5 Base template + home page + static files — [`phases/01-5-base-template.md`](phases/01-5-base-template.md)

## Phase 2 — Auth & Profile *(coming next)*
- [ ] 2.1 User registration view + form
- [ ] 2.2 Login / logout views
- [ ] 2.3 Password reset flow
- [ ] 2.4 `Profile` model (display name, bio, timezone, avatar)
- [ ] 2.5 Profile view + edit page
- [ ] 2.6 Access control: all notes/calendar pages require login

## Phase 3 — Notes CRUD
- [ ] 3.1 `Note` model (title, body, created_at, owner, day)
- [ ] 3.2 "New note" notepad-style page
- [ ] 3.3 Save with current local time → persist
- [ ] 3.4 Note detail (view/edit/delete)
- [ ] 3.5 Owner-only permissions

## Phase 4 — Calendar views
- [ ] 4.1 Monthly calendar with per-day note counts
- [ ] 4.2 Day view (24-hour grid)
- [ ] 4.3 Click a note block → detail
- [ ] 4.4 Month navigation (prev / next / today)
- [ ] 4.5 Timezone correctness (store UTC, render user tz)

## Phase 5 — AI day summarizer *(AI engineering focus)*
- [ ] 5.1 `Summary` model (day, owner, model_id, prompt_version, body, input_hash)
- [ ] 5.2 `ai/summarizer.py` service module (clean interface)
- [ ] 5.3 Versioned prompt template file
- [ ] 5.4 Structured output (JSON) + Pydantic validation
- [ ] 5.5 "Summarize this day" button
- [ ] 5.6 Multiple summaries per day
- [ ] 5.7 Summary markers on monthly view
- [ ] 5.8 Observability (tokens, latency, cost, cache hit/miss)
- [ ] 5.9 Tiny eval harness (pytest)

## Phase 6 — Quality & Ship
- [ ] 6.1 `ruff` + `black` + pre-commit hooks (incl. `gitleaks` for secret scanning)
- [ ] 6.2 Pytest setup + a handful of real tests per app
- [ ] 6.3 GitHub Actions: lint + test on PR
- [ ] 6.4 README with setup, env vars, screenshots
- [ ] 6.5 Dockerfile + `docker-compose.yml` (web + Postgres)
- [ ] 6.6 Deploy to **AWS**: ECS Fargate + RDS + S3 + Secrets Manager + GitHub Actions → ECR → ECS. Terraform (stretch).

## Phase 7 — Stretch / AI bonus
- [ ] 7.1 Embeddings over notes + semantic search
- [ ] 7.2 "Ask my notes" chat endpoint
- [ ] 7.3 Weekly / monthly rollup summaries
- [ ] 7.4 Streaming summarizer responses

---

## Definition of done (every feature)
- [ ] Code written and runs locally
- [ ] Migrations created and applied (if DB touched)
- [ ] At least one test (or an explicit note why skipped)
- [ ] Linter clean (from Phase 6.1 onward)
- [ ] Committed on its own feature branch
- [ ] PR opened, self-reviewed, merged, branch deleted

---

## Why this order?
- **Phase 1** gives you the muscle memory of the dev loop (edit → check → runserver → commit → PR → merge) on trivial changes before the stakes rise.
- **Phase 2** unlocks user context — every later feature assumes a logged-in user.
- **Phase 3** lets you create and read your own data; the simplest useful app.
- **Phase 4** adds a non-trivial view (the calendar) and forces you to confront timezones early.
- **Phase 5** is the AI-engineering spine of the project — isolated behind a clean interface so you can swap models and strategies without touching the rest.
- **Phase 6** turns a working app into a *shippable* one — the "engineering" in software engineering.
- **Phase 7** is portfolio-grade stretch work.
