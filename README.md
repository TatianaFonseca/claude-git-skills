# claude-git-skills

A collection of Claude Code skills to streamline your git workflow using [Conventional Commits](https://www.conventionalcommits.org).

## Skills included

| Skill | What it does |
|-------|-------------|
| [`smart-branch`](#smart-branch) | Generates a branch name before you start working |
| [`smart-commit`](#smart-commit) | Generates a commit message when you're ready to commit |

More skills coming soon: `smart-pr`, `changelog`.

---

## Installation

### Requirements
- [Claude Code](https://claude.ai/code) installed

### Install all skills at once

```bash
git clone https://github.com/TatianaFonseca/claude-git-skills
cp -r claude-git-skills/smart-branch ~/.claude/skills/
cp -r claude-git-skills/smart-commit ~/.claude/skills/
```

That's it. Claude Code picks them up automatically — no restart needed.

### Install a single skill

```bash
git clone https://github.com/TatianaFonseca/claude-git-skills
cp -r claude-git-skills/smart-commit ~/.claude/skills/   # or smart-branch
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

## Uninstall

```bash
rm -rf ~/.claude/skills/smart-branch
rm -rf ~/.claude/skills/smart-commit
```
