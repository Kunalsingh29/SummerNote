# Tutorial Index

Work through phases in order. Each one takes 15–60 minutes if you type everything yourself (which you should).

## Phase 0 — Prerequisites
- [`phases/00-prereqs.md`](phases/00-prereqs.md) — tools you need installed before Phase 1.

## Phase 1 — Foundation
- [`phases/01-1-scaffold-django.md`](phases/01-1-scaffold-django.md) — create the Django project and first app
- [`phases/01-2-git-init.md`](phases/01-2-git-init.md) — initialize Git with a safe `.gitignore`
- [`phases/01-3-github-repo.md`](phases/01-3-github-repo.md) — publish to GitHub using `gh`
- [`phases/01-4-settings-env.md`](phases/01-4-settings-env.md) — move settings to a `.env` file (12-factor config)
- [`phases/01-5-base-template.md`](phases/01-5-base-template.md) — base template, home page, static files, first URL route

## Phase 2 — Auth & Profile
- [`phases/02-1-signup.md`](phases/02-1-signup.md) — accounts app, SignupForm, auto-login
- [`phases/02-2-login-logout.md`](phases/02-2-login-logout.md) — built-in LoginView/LogoutView, session cookies, auth-aware nav
- [`phases/02-3-password-reset.md`](phases/02-3-password-reset.md) — the four-view flow + dev console email backend
- [`phases/02-4-profile-model.md`](phases/02-4-profile-model.md) — Profile sidecar model + post_save signal + Django admin
- [`phases/02-5-profile-view.md`](phases/02-5-profile-view.md) — @login_required profile view, ModelForm with timezone choices
- [`phases/02-6-access-control.md`](phases/02-6-access-control.md) — auth-branching home, conventions doc, first tests

## Phase 3 — Notes CRUD (coming soon)
## Phase 4 — Calendar views (coming soon)
## Phase 5 — AI day summarizer (coming soon)
## Phase 6 — Quality, Docker, deploy to AWS (coming soon)

---

## The phase file template

Every phase file follows the same structure so you always know where to look:

1. **🎯 Goal** — one sentence.
2. **🧠 Concepts you'll meet** — the ideas introduced in this phase.
3. **🤔 Why we're doing this now** — placing the step in context.
4. **🗺 Flow** — a flowchart of the step.
5. **⌨️ Commands — you type these** — each command with explanation, expected output, and troubleshooting.
6. **✅ Validation** — a checklist of concrete things that must be true before you move on.
7. **🧪 Try breaking it (optional)** — small experiments that build intuition by causing errors on purpose.
8. **📝 Reflect** — questions to answer in your own words.
9. **🔗 Deeper reading** — official docs for anything you want to go further on.
10. **➡️ Next** — pointer to the next phase file.

See [`templates/phase.md`](templates/phase.md) for the empty template.

## Conventions used in the docs

- `# This is a comment` inside a code block means "don't type this, it's for you."
- Placeholders look like `<ANGLE_BRACKETS>` — replace with your real value.
- Paths are usually written as `~/path/to/SummerNote` — meaning *wherever you cloned this repo on your machine*.
- Prompts shown as `$ some command` — don't type the `$`.

## If something breaks

1. Read the error message slowly. Most errors tell you exactly what's wrong.
2. Check that phase's **Troubleshooting** section.
3. Google the exact error (not a paraphrase).
4. Only then ask an LLM. If you do, paste the exact command, the exact error, and what you've already tried.
