# Phase 0 — Prerequisites

## 🎯 Goal
Verify your machine has every tool Phase 1 will need, and understand what each tool is for before you start using it.

## 🧠 Concepts you'll meet
- Package manager (Homebrew on macOS, apt on Linux, winget on Windows)
- Python interpreter and `pip`
- Git — distributed version control
- GitHub CLI (`gh`) — command-line interface to github.com
- Shell basics (you'll live in zsh/bash)

## 🤔 Why we're doing this now
Nothing is more frustrating than being halfway through a step and hitting *"command not found"*. Verify once, build with confidence.

## 🗺 Flow

```mermaid
flowchart LR
    A[Check Python] --> B[Check Git] --> C[Check gh] --> D[Install gh if missing] --> E[gh auth login] --> F[Ready]
```

## ⌨️ Commands — you type these

Open your terminal and run each command. **Read the expected output before moving on.**

### 1. Check Python version

```bash
python3 --version
```

**What this does:** prints the Python 3 version. Django 5 requires Python 3.10 or newer.
**Expected output (something like):**
```
Python 3.12.5
```
**If something went wrong:**
- `command not found: python3` → Install Python 3 from https://www.python.org/downloads/ (macOS), or your distro's package manager.
- Version is older than 3.10 → Install a newer Python. Keep the old one; use `python3.12` explicitly.

### 2. Check pip

```bash
python3 -m pip --version
```

**What this does:** confirms `pip` (Python's package installer) is available via the standard `python -m pip` invocation.
**Expected output:**
```
pip 24.x from /some/path/pip (python 3.12)
```

### 3. Check Git

```bash
git --version
```

**Expected output (anything 2.x works):**
```
git version 2.47.0
```
**If missing:** macOS installs Git on first use when you run `git --version`; or `brew install git`. Linux: `sudo apt install git`.

### 4. Set a Git identity (once per machine)

Git needs to know who's making commits. Replace the placeholders with your real name and email you want to appear in commit history (your GitHub-noreply email is fine for privacy).

```bash
git config --global user.name "<YOUR NAME>"
git config --global user.email "<YOUR EMAIL>"
git config --global init.defaultBranch main
```

**Verify:**
```bash
git config --global --list | grep -E 'user|init'
```

### 5. Check / install the GitHub CLI (`gh`)

```bash
gh --version
```

**If missing (macOS):**
```bash
brew install gh
```

**Linux / Windows:** see https://cli.github.com/

### 6. Authenticate `gh` with your GitHub account

```bash
gh auth login
```

**Pick:**
- *Where?* → **GitHub.com**
- *Preferred protocol?* → **HTTPS**
- *Authenticate Git with your GitHub credentials?* → **Yes**
- *How to authenticate?* → **Login with a web browser**

Copy the 8-character code shown in your terminal, press **Enter** to open the browser, paste the code, authorize.

**Confirm success:**
```bash
gh auth status
```
**Expected output includes:**
```
✓ Logged in to github.com as <your-github-username>
```

### 7. Pick a project folder on disk

Decide where SummerNote will live on your machine. Pick a path you'll remember. For the rest of these docs we'll refer to it as `~/path/to/SummerNote`.

```bash
cd ~/path/to/where/you/keep/projects
# clone this repo if you haven't:
# gh repo clone <your-github-username>/SummerNote
cd SummerNote
pwd
```

**Verify:**
```bash
ls
```
You should see `README.md`, `docs/`, `.gitignore`.

## ✅ Validation — you've finished this phase when
- [ ] `python3 --version` prints 3.10 or newer
- [ ] `python3 -m pip --version` works
- [ ] `git --version` works
- [ ] `git config --global user.name` returns your name
- [ ] `gh --version` works
- [ ] `gh auth status` says *Logged in to github.com*
- [ ] You can `cd` into your local SummerNote folder and `ls` shows `README.md` and `docs/`

## 🧪 Try breaking it (optional)
1. Run `python --version` (no 3). What happens? On many Macs it says `command not found`, because macOS doesn't alias `python` to `python3`. **Lesson:** always use `python3` on macOS unless you're inside a venv (coming next phase).

## 📝 Reflect
- In your own words: why does Git need to be configured with a name and email before you can commit?
- `gh auth login` stored a credential somewhere. Where would you guess, and why isn't it just a password file in your home directory?

## 🔗 Deeper reading
- Python downloads: https://www.python.org/downloads/
- Git tutorial: https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control
- GitHub CLI manual: https://cli.github.com/manual/

## ➡️ Next
`01-1-scaffold-django.md`
