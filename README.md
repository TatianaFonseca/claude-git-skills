# claude-git-skills

A collection of Claude Code skills to streamline your git workflow using [Conventional Commits](https://www.conventionalcommits.org).

## Skills included

| Skill | What it does |
|-------|-------------|
| [`smart-branch`](#smart-branch) | Generates a branch name before you start working |
| [`smart-commit`](#smart-commit) | Generates a commit message when you're ready to commit |
| [`smart-pr`](#smart-pr) | Generates a PR title and description from your branch commits |
| [`smart-changelog`](#smart-changelog) | Generates or updates CHANGELOG.md from your commit history |

---

## Installation

### Requirements
- [Claude Code](https://claude.ai/code) installed

### Install all skills at once

```bash
git clone https://github.com/TatianaFonseca/claude-git-skills
cp -r claude-git-skills/smart-branch ~/.claude/skills/
cp -r claude-git-skills/smart-commit ~/.claude/skills/
cp -r claude-git-skills/smart-pr ~/.claude/skills/
cp -r claude-git-skills/smart-changelog ~/.claude/skills/
```

That's it. Claude Code picks them up automatically — no restart needed.

### Install a single skill

```bash
git clone https://github.com/TatianaFonseca/claude-git-skills
cp -r claude-git-skills/smart-commit ~/.claude/skills/   # replace with any skill name
```

---

## The workflow

```
smart-branch  →  (work)  →  smart-commit  →  smart-pr  →  smart-changelog
     🌿                           💬               🔗              📋
```

---

## smart-branch

Generates a conventional branch name and switches to it after your confirmation.

**Triggers on:** "quiero crear una rama", "create a branch for", "nueva rama para", "voy a trabajar en X"

**Format:** `type/short-description-in-kebab-case`

**Examples:**
```
feat/add-guest-checkout
fix/auth-session-expiry
hotfix/payment-timeout
refactor/user-profile-screen
chore/update-dependencies
```

**Flow:**
1. You describe what you're working on
2. Claude proposes a branch name
3. You confirm
4. Claude runs `git checkout -b` — never before your ok

---

## smart-commit

Generates a strict conventional commit message and commits after your confirmation.

**Triggers on:** "commit this", "quiero hacer un commit", "subir mis cambios", "I want to push"

**Format:** `type(scope): short description`

**Examples:**
```
feat(checkout): add coupon field for registered users
fix(auth): redirect to login when session expires
chore(deps): bump react-navigation to v6.8.1
```

**Flow:**
1. Claude reads your staged changes
2. If changes are unrelated → suggests splitting into separate commits
3. Generates the message and shows it to you
4. You confirm
5. Claude commits — never before your ok

---

## smart-pr

Analyzes all commits in your branch, generates a PR title and description, then opens the PR after your confirmation.

**Triggers on:** "quiero abrir un PR", "create a PR", "open pull request", "subir para review"

**PR description includes:**
- What changed
- Why
- List of changes by module
- How to test

**Example:**
```
feat(checkout): add guest checkout flow

## What changed
Added support for guest checkout, allowing users to complete a purchase without an account.

## Why
Users were dropping off at the sign-up step during checkout.

## Changes
- feat(checkout): add guest option on the cart screen
- fix(order): handle missing user ID for guest orders

## How to test
1. Add an item to your cart without logging in
2. Tap checkout and verify the guest option appears
3. Complete a test purchase
```

**Flow:**
1. Claude reads all commits ahead of main
2. Generates title + description
3. You confirm
4. Claude runs `gh pr create` — never before your ok

---

## smart-changelog

Reads your commit history and generates or updates `CHANGELOG.md` in the project root.

**Triggers on:** "update changelog", "generate changelog", "actualizar changelog", "release notes"

**Format:** [Keep a Changelog](https://keepachangelog.com) standard

**Example output:**
```markdown
## [1.3.0] - 2026-03-11

### Features
- feat(checkout): add guest checkout flow
- feat(profile): allow users to upload a profile picture

### Bug Fixes
- fix(auth): redirect to login when session expires

### Maintenance
- chore(deps): bump react-navigation to v6.8.1
```

**Flow:**
1. Claude reads commits since the last tag (or all if no tags)
2. Groups changes by type
3. Shows you the new entry
4. You confirm
5. Claude writes to CHANGELOG.md — never before your ok

---

## Uninstall

```bash
rm -rf ~/.claude/skills/smart-branch
rm -rf ~/.claude/skills/smart-commit
rm -rf ~/.claude/skills/smart-pr
rm -rf ~/.claude/skills/smart-changelog
```
