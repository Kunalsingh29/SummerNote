# Phase 1.4 — Settings split + `.env` loading (your first feature branch)

## 🎯 Goal
Move `SECRET_KEY`, `DEBUG`, and `ALLOWED_HOSTS` out of `settings.py` and into a `.env` file read at runtime. Register the `notes` app with Django. Practice the full feature-branch → commit → PR → merge loop.

## 🧠 Concepts you'll meet
- **12-factor Config principle** — same code, different environments, zero code changes.
- **`python-decouple`** — tiny library that reads `.env` or real env vars uniformly.
- **Fail-loud secrets** — missing required env vars should crash at boot, not run insecurely.
- **`INSTALLED_APPS`** — the registry that makes an app "real" to Django.
- **Feature-branch workflow** — branch → push → PR → merge → delete.

## 🤔 Why we're doing this now
Phase 1.1 scaffolded a Django project whose `SECRET_KEY` is hardcoded in `settings.py` and whose `notes` app isn't actually registered. Both are landmines: hardcoded secrets leak the moment the repo goes public, and an unregistered app's models/migrations are invisible to Django. Fix both before writing a single line of real feature code.

## 🗺 Flow

```mermaid
flowchart TD
    A[git checkout -b feature/1.4-settings-env] --> B[pip install python-decouple]
    B --> C[generate SECRET_KEY]
    C --> D[create .env with real values]
    D --> E[edit settings.py:<br/>config() calls + notes in INSTALLED_APPS]
    E --> F[manage.py check + migrate + runserver smoke test]
    F --> G[git status  review]
    G --> H[git commit, push, open PR]
    H --> I[self-review diff, merge, delete branch]
```

## ⌨️ Commands — you type these

### 1. Create a feature branch

```bash
git checkout -b feature/1.4-settings-env
```

**Verify:**
```bash
git branch --show-current
```
→ `feature/1.4-settings-env`

### 2. Install `python-decouple` and update requirements

```bash
pip install python-decouple
pip freeze > requirements.txt
```

**Verify** `python-decouple==3.x` is now in `requirements.txt`:
```bash
grep decouple requirements.txt
```

### 3. Generate a strong `SECRET_KEY`

```bash
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

**Expected:** a 50-character string of letters, digits, symbols. **Copy it** — you'll paste it into `.env` in the next step.

### 4. Create your real `.env` (NEVER commit this)

```bash
cat > .env <<EOF
DJANGO_SECRET_KEY=<PASTE-THE-GENERATED-KEY-HERE>
DJANGO_DEBUG=true
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1
EOF
```

**Replace** `<PASTE-THE-GENERATED-KEY-HERE>` with the key from step 3. Open the file to verify:
```bash
cat .env
```

**Sanity-check it's gitignored:**
```bash
git status
```
`.env` must **not** appear in the output. If it does, your `.gitignore` is missing the `.env` entry — fix immediately.

### 5. Edit `summernote/settings.py`

Open the file in your editor. Find this block near the top:

```python
from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent


# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/5.2/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-...'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []
```

**Replace it with:**

```python
from pathlib import Path

from decouple import Csv, config

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

# All environment-driven settings live together, right at the top,
# so a reader can see the config contract at a glance.
# Values come from a local .env file (dev) or the real environment (prod).
SECRET_KEY = config('DJANGO_SECRET_KEY')  # no default: fail loud if missing
DEBUG = config('DJANGO_DEBUG', default=False, cast=bool)
ALLOWED_HOSTS = config('DJANGO_ALLOWED_HOSTS', default='', cast=Csv())
```

**What changed:**
- `from decouple import Csv, config` — the library we just installed.
- `SECRET_KEY = config('DJANGO_SECRET_KEY')` — no default, so a missing env var crashes at boot (what you want for a secret).
- `DEBUG` defaults to `False` — safe default: a prod container missing this var won't accidentally run in debug mode.
- `ALLOWED_HOSTS` defaults to empty — safe default: Django will refuse all hosts, forcing you to configure it explicitly.

### 6. Register the `notes` app

Still in `settings.py`, find:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

**Add our app at the end:**

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Local apps
    'notes',
]
```

### 7. Verify Django is happy

```bash
python manage.py check
```
**Expected:** `System check identified no issues (0 silenced).`

**If you see** `ImproperlyConfigured: Set the DJANGO_SECRET_KEY environment variable`:
→ Your `.env` file is missing or doesn't have `DJANGO_SECRET_KEY=`. Fix it.

### 8. Apply Django's initial migrations (creates the local SQLite DB)

```bash
python manage.py migrate
```
**Expected:** a long list ending in `Applying sessions.0001_initial... OK`.

A new file `db.sqlite3` appears. It's gitignored.

### 9. Smoke test the dev server

```bash
python manage.py runserver
```
**Expected output:**
```
Starting development server at http://127.0.0.1:8000/
```
Open http://127.0.0.1:8000/ in a browser. You should see Django's green "The install worked successfully!" rocket page.

**Stop the server** with `Ctrl+C`.

### 10. Review what you're about to commit

```bash
git status
```

**Expected modifications:**
- `summernote/settings.py`
- `requirements.txt`

**Expected untracked (should NOT be listed because of `.gitignore`):**
- `.env` ← verify absence!
- `db.sqlite3` ← verify absence!
- `.venv/`

If `.env` or `db.sqlite3` shows up here, STOP and fix `.gitignore` before committing.

### 11. Commit

```bash
git add summernote/settings.py requirements.txt
git commit -m "feat(1.4): load settings from env; register notes app"
```

### 12. Push the branch

```bash
git push -u origin feature/1.4-settings-env
```

**Expected output includes:**
```
remote: Create a pull request for 'feature/1.4-settings-env' on GitHub by visiting:
remote:      https://github.com/<your-username>/SummerNote/pull/new/feature/1.4-settings-env
```

### 13. Open a pull request

Copy the URL printed above into your browser (or run `gh pr create` to open one from the CLI):

```bash
gh pr create --base main --head feature/1.4-settings-env \
  --title "feat(1.4): env-driven settings + register notes app" \
  --body "Phase 1.4 — move SECRET_KEY/DEBUG/ALLOWED_HOSTS to env via python-decouple; add notes to INSTALLED_APPS."
```

### 14. Self-review your diff on GitHub

In the PR page: click **Files changed**. Read every line of the diff. Ask yourself:
- Does any secret value appear here?
- Is anything in the diff that I don't understand?

If both answers are no → **Merge pull request** → **Confirm merge** → **Delete branch**.

### 15. Sync local `main`

```bash
git checkout main
git pull --prune
git branch -d feature/1.4-settings-env
```

**Verify:**
```bash
git log --oneline
```
→ Should show your merge commit on top.

## ✅ Validation — you've finished this phase when
- [ ] `.env` exists locally with a real `DJANGO_SECRET_KEY`
- [ ] `.env` is NOT committed (`git ls-files | grep '^\.env$'` returns nothing)
- [ ] `python manage.py check` → *0 issues*
- [ ] `python manage.py runserver` → Django rocket page at http://127.0.0.1:8000/
- [ ] PR for `feature/1.4-settings-env` is merged into `main` on GitHub
- [ ] Local `main` is up to date with the merged change
- [ ] The feature branch is deleted locally and on GitHub

## 🧪 Try breaking it (optional)
1. Temporarily rename your `.env` to `.env.bak` and run `python manage.py check`. You should see `decouple.UndefinedValueError`. Rename it back. **Lesson:** fail-loud works.
2. Temporarily set `DJANGO_DEBUG=whatever` in `.env` and run `check`. You'll get a cast error. Change to `true` or `false`. **Lesson:** `cast=bool` is strict for a reason.
3. Try committing `.env` on purpose: `git add -f .env`, `git status`, see it staged, then **unstage** with `git restore --staged .env`. Never commit a secret.

## 📝 Reflect
- In your own words: what's the 12-factor Config principle, and why does `.env.example` help a teammate but not leak your secrets?
- Why did we deliberately give `SECRET_KEY` **no default** but `DEBUG` a safe default of `False`?
- You just did your first feature-branch PR. Why is the self-review step (reading your own diff) worth doing on a solo project?

## 🔗 Deeper reading
- 12-factor Config: https://12factor.net/config
- `python-decouple` docs: https://pypi.org/project/python-decouple/
- Django settings reference: https://docs.djangoproject.com/en/5.2/ref/settings/

## ➡️ Next
`01-5-base-template.md` — replace Django's default page with your own home page using template inheritance.
