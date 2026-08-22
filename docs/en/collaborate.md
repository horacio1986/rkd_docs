# 🚀 Contribution Guide for Rocketdoo

Thank you for your interest in contributing to **Rocketdoo**! This guide describes the complete path a change travels: from spotting a bug or proposing an improvement, to seeing that change published on PyPI.

The official repository is **[HDM-soft/rocketdoo](https://github.com/HDM-soft/rocketdoo)**.

---

## Table of contents

- [Golden rules](#golden-rules)
- [Branching model](#branching-model)
- [The path of a contribution](#the-path-of-a-contribution)
- [Step 1 — Open the issue](#step-1-open-the-issue)
- [Step 2 — Set up the development environment](#step-2-set-up-the-development-environment)
- [Step 3 — Create the working branch](#step-3-create-the-working-branch)
- [Step 4 — Develop and commit](#step-4-develop-and-commit)
- [Step 5 — Test before opening the PR](#step-5-test-before-opening-the-pr)
- [Step 6 — Open the Pull Request against `dev/v3`](#step-6-open-the-pull-request-against-devv3)
- [Step 7 — Review and merge](#step-7-review-and-merge)
- [Step 8 — Promotion to production](#step-8-promotion-to-production)
- [Versioning and releases](#versioning-and-releases)
- [Urgent hotfix](#urgent-hotfix)
- [Code conventions](#code-conventions)
- [Common problems](#common-problems)

---

## Golden rules

Before writing a single line of code, five rules that shape the whole process:

1. **Every task starts with an issue.** No issue, no branch. The issue number names the branch and appears in the commits and in the PR.
2. **Never work directly on `main`, `test/v3` or `dev/v3`.** Always a short-lived branch created from `dev/v3`.
3. **No contribution PR targets `main`.** Changes enter through `dev/v3`, and maintainers promote them to `test/v3` and then to `main`.
4. **Contributors don't touch the version.** The `pyproject.toml` bump is done by the maintainer cutting the release, in a dedicated commit. See [Versioning and releases](#versioning-and-releases).
5. **One PR = one self-contained change.** If you're solving two distinct problems, that's two issues and two branches.

---

## Branching model

```
main              ← stable version · merging a PR here publishes to PyPI (CI)
  ↑ release PR (opened by a maintainer)
test/v3           ← QA gate: the package is installed from this branch and smoke-tested
  ↑ tested-integration PR (opened by a maintainer)
dev/v3            ← development integration · base of every working branch
  ↑ your contribution PR
fix/123-slug · feat/124-slug · mig/125-slug · chore/126-slug
```

Promotion path of any change:

```
<type>/N-slug  →  dev/v3  →  test/v3  →  main  →  PyPI
```

> `test/v3` is the **intermediate branch** between development and production. It exists to test the **installed package** (not the local repo): a change that works in your clone can still fail once it's built and installed with `pipx`. That filter is what keeps a broken version from reaching PyPI.

`dev/v2` is the previous, already stabilized line; it only receives occasional back-ports.

---

## The path of a contribution

This is the full journey, end to end:

```
issue #123  ─────────────────────────────────────────────────────────────────
  │  describes the problem: repro steps, expected vs. actual, environment
  │
  ├─ branch  fix/123-short-description       created from dev/v3
  │          commits "FIX: ..." with "Refs #123" in the body
  │          local test: pipx install + rkd scaffold in an empty folder
  │
  ├─ PR → dev/v3        "Refs #123"   → maintainer review → merge
  │
  ├─ PR → test/v3       QA gate: installed from the branch, checklist run
  │
  └─ PR → main          "Closes #123" → CI builds and publishes to PyPI
                        → tag vX.Y.Z + GitHub Release → the issue is closed
```

The first three blocks are your responsibility as a contributor. The last two are executed by maintainers, but it helps to know them to understand why certain things are asked for in the PR.

---

## Step 1 — Open the issue

The issue is the starting point: it provides context, lets the approach be discussed before you invest time in code, and gives the branch its name.

### 1.1 Search for duplicates

```bash
# With GitHub CLI
gh issue list --repo HDM-soft/rocketdoo --search "unpack filestore" --state all -L 10
```

Or from the web: [repository issues](https://github.com/HDM-soft/rocketdoo/issues).

### 1.2 Reproduce the problem in a clean environment

Rocketdoo generates the whole project structure with `rkd scaffold`, so the reproduction must happen in an **empty folder**, using the published version:

```bash
mkdir -p /tmp/rkd-repro && cd /tmp/rkd-repro
rkd --version        # write down the exact version
rkd scaffold
rkd init
# ... the command that fails
```

Write down: the exact command, the full output (or traceback), the Rocketdoo version, your operating system, and your Python and Docker versions.

### 1.3 Create the issue

```bash
gh issue create --repo HDM-soft/rocketdoo \
  --title "FIX: rkd unpack does not restore the filestore when the volume is slow to mount" \
  --label bug \
  --body-file /tmp/issue.md
```

Or from the web with the **New issue** button.

**Title:** a single line, using the same prefix the final commit will carry (`FIX:`, `FEAT:`, `MIG:`, `DOC:`, `REF:`), describing the symptom for a bug or the expected outcome for an improvement.

**Suggested labels:** `bug`, `enhancement`, `migration`, `docs`, `ci`, `v3`, `priority:high`.

### 1.4 Template — bug report

```markdown
## Description
<what fails, in one or two sentences>

## Steps to reproduce
1. `mkdir /tmp/rkd-test && cd /tmp/rkd-test`
2. `rkd scaffold`
3. `rkd init` (values used: Odoo 17.0, Community, no gitman, ports 8069/8888)
4. `rkd unpack`

## Expected behavior
<what should happen>

## Actual behavior
<what happens; paste the traceback or relevant output in a code block>

## Environment
- rocketdoo: `rkd --version` → 3.1.4
- Python: 3.12.3
- OS: Ubuntu 24.04 (WSL2)
- Docker: 27.x / Compose v2.x

## Impact
<blocking | degraded | cosmetic>
```

### 1.5 Template — improvement or feature

```markdown
## Current problem or limitation
<what can't be done today, and who it affects>

## Proposal
<proposed behavior; if it adds a command or option, show the interface>

## Alternatives considered
<discarded options and why>

## Acceptance criteria
- [ ] <verifiable condition 1>
- [ ] <verifiable condition 2>
- [ ] Documented in the README and in the documentation

## Versioning impact
<patch | minor | major> — <justification>
```

**Keep the issue number.** It's the `N` you'll use in the branch, the commits and the PR.

---

## Step 2 — Set up the development environment

### Prerequisites

- Python 3.10 or higher (matching `requires-python` in `pyproject.toml`)
- Git
- Up-to-date `pip` (`pip install --upgrade pip`)
- `pipx` (recommended for testing the CLI)
- GitHub CLI (`gh`) — optional, but it simplifies issues and PRs

### 1. Fork the repository

On GitHub, click **Fork** in the top-right corner of [HDM-soft/rocketdoo](https://github.com/HDM-soft/rocketdoo).

### 2. Clone your fork locally

```bash
git clone https://github.com/YOUR_USERNAME/rocketdoo.git
cd rocketdoo
```

### 3. Add the original repository as a remote

```bash
git remote add upstream https://github.com/HDM-soft/rocketdoo.git
```

This lets you sync with changes from the original repo whenever needed:

```bash
git fetch upstream
git merge upstream/dev/v3
```

### 4. Development virtual environment

To modify the code and see the changes reflected immediately, use a dedicated virtual environment:

```bash
# From the root of the cloned repo
python -m venv venv-dev

# Linux / macOS
source venv-dev/bin/activate
# Windows
.\venv-dev\Scripts\activate
```

---

## Step 3 — Create the working branch

Always from an up-to-date `dev/v3`, never from `main`:

```bash
git fetch upstream
git checkout dev/v3
git pull --ff-only upstream dev/v3
```

**Naming:** `<type>/<issue-number>-<short-slug>`

```bash
git checkout -b fix/123-unpack-filestore
# Examples:
# git checkout -b feat/124-multi-model-support
# git checkout -b mig/125-odoo-18
# git checkout -b chore/126-update-dependencies
```

If you use GitHub CLI, this command creates the branch **already linked to the issue** on GitHub:

```bash
gh issue develop 123 --base dev/v3 --name fix/123-unpack-filestore --checkout
```

### Branch types

| Type | When to use it | Base |
|------|---------------|------|
| `fix/` | Bug fix | `dev/v3` |
| `feat/` | New feature or command | `dev/v3` |
| `mig/` | Migration (Odoo version, dependency, config format) | `dev/v3` |
| `chore/` | Maintenance (deps, CI, cleanup) | `dev/v3` |
| `docs/` | Documentation only | `dev/v3` |
| `refactor/` | Refactoring without behavior change | `dev/v3` |
| `hotfix/` | Emergency with production broken | `main` |

---

## Step 4 — Develop and commit

One commit per logical change, with uppercase prefixes (the ones used by the project history):

| Prefix | When to use it |
|--------|---------------|
| `FIX:` | Bug fix |
| `FEAT:` | New feature or command |
| `UPD:` | General update (CI, config, docs bundled with code) |
| `UPG:` | **Version bump only** (`UPG: version 3.1.5`) — reserved for maintainers |
| `REF:` | Refactoring without behavior change |
| `DOC:` | Documentation |
| `TEST:` | Adding or fixing tests |
| `DEL:` | Removing code or artifacts |

The issue is referenced in the commit **body**:

```bash
git commit -m "FIX: wait for the volume mount before restoring the filestore

Refs #123"
```

Use `Refs #123` in intermediate commits. `Closes #123` is reserved for the release PR into `main`, because GitHub only auto-closes issues when a PR is merged into the default branch.

---

## Step 5 — Test before opening the PR

### 5.1 Install your local code and run the minimum cycle

```bash
pipx install /path/to/your/rocketdoo/clone --force

mkdir -p /tmp/rkd-test && cd /tmp/rkd-test
rkd scaffold
rkd init
# ... and the command your change touches
```

### 5.2 Pre-flight of the CI gate

The continuous integration workflow **fails** if `flake8` finds syntax errors or undefined names. Run the same check before pushing:

```bash
source venv-dev/bin/activate
flake8 rocketdoo --count --select=E9,F63,F7,F82 --show-source --statistics
python -c "import rocketdoo, rocketdoo.cli"
```

If that command reports errors, the PR will go red.

### 5.3 Test a branch without touching your production install

To review someone else's branch (or your own, already pushed) without uninstalling your working Rocketdoo:

#### Option A — With `pipx` (recommended for the CLI) ✅

`pipx` installs into an isolated environment and exposes the commands globally. With `--suffix`, the production version and the test version coexist:

```bash
pipx install "git+https://github.com/HDM-soft/rocketdoo.git@fix/123-unpack-filestore" --suffix=-test
```

That gives you two commands in parallel:

```bash
rkd             # ← production version installed from PyPI (untouched)
rkd-test        # ← version from the test branch
```

> The suffix applies to **every** command the package exposes, so `rocketdoo` and `rocketdoo-test` are available too. For `pipx`, the installed package is named `rocketdoo-test`.

To update after new commits are pushed to the branch, and to clean up when you're done:

```bash
pipx reinstall rocketdoo-test
pipx uninstall rocketdoo-test
```

#### Option B — With `pip` inside a `venv` (for imports or scripts)

```bash
mkdir test-rocketdoo && cd test-rocketdoo
python -m venv venv-test
source venv-test/bin/activate   # Linux / macOS
# .\venv-test\Scripts\activate  # Windows

pip install "git+https://github.com/HDM-soft/rocketdoo.git@fix/123-unpack-filestore"

# update after new commits
pip install --force-reinstall "git+https://github.com/HDM-soft/rocketdoo.git@fix/123-unpack-filestore"
```

#### Private repo: authentication with a token

```bash
pip install "git+https://YOUR_TOKEN@github.com/HDM-soft/rocketdoo.git@fix/123-unpack-filestore"
```

| Method | When to use it |
|--------|---------------|
| `pipx install git+... --suffix=-test` | **Recommended.** Test CLI commands without touching the production install |
| `pipx install /local/path --force` | Test your own code before pushing |
| `pip install git+...` inside a venv | Test Python imports or script integrations |

---

## Step 6 — Open the Pull Request against `dev/v3`

```bash
git push -u origin fix/123-unpack-filestore

gh pr create --repo HDM-soft/rocketdoo \
  --base dev/v3 --head YOUR_USERNAME:fix/123-unpack-filestore \
  --title "FIX: wait for the volume mount before restoring the filestore" \
  --body-file /tmp/pr.md
```

Or from the web with the **Compare & pull request** button, making sure the target is `dev/v3`.

> **The PR targets `dev/v3`, never `main` or `test/v3`.** A PR against `main` triggers the PyPI publishing workflow and will be closed without merging.

### PR template

````markdown
## What changes
<summary in 2-4 lines>

Refs #123

## Why
<root cause of the bug, or need for the improvement>

## How to test it
```bash
pipx install "git+https://github.com/YOUR_USERNAME/rocketdoo.git@fix/123-unpack-filestore" --suffix=-test
mkdir /tmp/rkd-pr && cd /tmp/rkd-pr
rkd-test scaffold && rkd-test init && rkd-test unpack
```

Expected result: <...>

## Verified
- [ ] The reproduction of #123 no longer happens
- [ ] `flake8 rocketdoo --select=E9,F63,F7,F82` clean
- [ ] `python -c "import rocketdoo, rocketdoo.cli"` OK
- [ ] Local pipx cycle OK
- [ ] The version in `pyproject.toml` was not modified
- [ ] No build artifacts were added to the repo
````

Use `Refs #123`, not `Closes #123`: auto-closing only happens on merge to `main`.

---

## Step 7 — Review and merge

Once the PR is open, **request a review** from the maintainers (the *Reviewers* field, or by mentioning them in a comment). No PR is merged without at least one approval.

What reviewers look at:

- The change solves what the issue describes, and nothing more.
- No version changes in `pyproject.toml`, `README.md` or `LICENSE`.
- No build artifacts slipped in (`dist/`, `build/`, `*.egg-info`).
- The `flake8` gate passes and the code respects PEP 8.
- The new behavior is documented if it changes how a command is used.
- There is a concrete way to reproduce and verify the fix.

If changes are requested: commit onto the **same branch** and push; the PR updates itself. Don't open a new PR.

```bash
git commit -m "FIX: apply review feedback

Refs #123"
git push
```

Once approved, a maintainer merges it into `dev/v3` (squashing if the branch has back-and-forth commits) and deletes the remote branch. From there your change travels along with the rest of the integration.

---

## Step 8 — Promotion to production

These two PRs are opened by maintainers. They're documented here so it's clear where every change ends up.

### 8.1 `dev/v3` → `test/v3` (QA gate)

Before opening this PR, the maintainer applies the version bump on `dev/v3`. After the merge, the package is tested **as installed from the branch**, not from the local repo:

```bash
pipx install "git+https://github.com/HDM-soft/rocketdoo.git@test/v3" --suffix=-test
rocketdoo-test --version
```

QA checklist: `scaffold`, `init`, `info`, `up/status/logs/down`, `mail on|off|status`, `traefik on|off|status`, `deploy init|list-modules`, `gui`, `pack`/`unpack` if the change touches them, and **the reproduction of every included issue, verifying it no longer happens**.

If something fails at this point, it's fixed on a new branch from `dev/v3` (back to [Step 3](#step-3-create-the-working-branch)), never on `test/v3` itself.

### 8.2 `test/v3` → `main` (release)

This PR does carry `Closes #123` (one line per included issue) plus the release notes. Merging it triggers `.github/workflows/publish-rkd.yml`, which runs the tests, builds the package and publishes it to PyPI via Trusted Publishing.

After publication, and manually: tag the merge commit (`git tag -a vX.Y.Z origin/main`), create the GitHub Release, close any issues that didn't close themselves, and back-merge `main` into `test/v3` and `dev/v3`.

---

## Versioning and releases

**If you're contributing, don't touch the version.** The bump is done by the maintainer cutting the release, on `dev/v3`, in a dedicated `UPG: version X.Y.Z` commit. This avoids conflicts between concurrent branches and duplicate bumps.

The version lives in **three** files and they must move together in the same commit:

| File | Field |
|------|-------|
| `pyproject.toml` | `version = "X.Y.Z"` — the packaging source of truth (what PyPI publishes) |
| `README.md` | the `## Version:` block |
| `LICENSE` | the `Versión:` line of the header (bump `Fecha:` too) |

`rocketdoo/__init__.py` is **never** edited: it reads the installed version at runtime through `importlib.metadata`, and the `__version__ = "dev"` there is only the fallback for a source checkout.

Semantic versioning:

| Change | Bump | Example |
|--------|------|---------|
| Bug fix, internal adjustment, documentation | patch | 3.1.4 → 3.1.5 |
| New command or option, backwards-compatible feature | minor | 3.1.5 → 3.2.0 |
| Breaking change (CLI, config format, generated layout) | major | 3.2.0 → 4.0.0 |

> `twine upload` is never run by hand against production PyPI: PyPI versions are immutable, and a manual upload makes the CI fail afterwards with `400 File already exists`. TestPyPI is used to rehearse a publication.

---

## Urgent hotfix

Only for broken production, when waiting for the QA gate isn't viable. The branch is created from `main`:

```bash
git fetch upstream
git checkout -b hotfix/130-slug upstream/main
```

The PR goes straight to `main`. Since it skips `test/v3`, local validation is **mandatory**: build the package, `twine check`, install the wheel with `pipx`, and verify the failure is resolved. After publication, back-merging `main` into `test/v3` and `dev/v3` is mandatory so the fix isn't lost.

---

## Code conventions

- We follow [PEP 8](https://pep8.org/) for Python code style.
- Use **type hints** wherever possible.
- All new code must include basic **docstrings**.
- If you add a feature, include at least one **test** covering it.
- Avoid introducing new dependencies without prior justification and discussion in the related issue.
- Don't commit build artifacts (`dist/`, `*.egg-info`) or local environment files.

---

## Common problems

| Symptom | Cause | Solution |
|---------|-------|----------|
| The PR went red on the `test` job | `flake8 --select=E9,F63,F7,F82` found a syntax error or an undefined name | Run the local check from [Step 5.2](#step-5-test-before-opening-the-pr) and fix the real error |
| The issue didn't close when the PR was merged | The PR merged into `dev/v3`, not the default branch | That's expected: it closes with the release PR to `main` |
| `rkd --version` shows `dev` | Package installed without metadata (editable checkout) | Reinstall with `pipx install ... --force` |
| Conflict in `pyproject.toml` when rebasing | Your branch touched the version | Discard that change: the bump doesn't belong in working branches |
| `rkd scaffold` fails in the test directory | The folder wasn't empty | Rocketdoo generates the structure from scratch; use a new folder |
| `gh: command not found` | GitHub CLI not installed | Install it from [cli.github.com](https://cli.github.com/) or run the whole flow from the web |

---

## Questions?

Open a [GitHub Discussion](https://github.com/HDM-soft/rocketdoo/discussions) or reach out to the project maintainers. Every contribution, no matter how small, is welcome!

---

*This document is part of the official Rocketdoo documentation.*
