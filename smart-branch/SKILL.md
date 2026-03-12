---
name: smart-branch
description: >
  Generates a branch name following conventional commits style prefixes and kebab-case.
  Use this skill whenever the user wants to create a new branch, start working on
  something new, or asks for a branch name. Triggers on phrases like "quiero crear
  una rama", "create a branch for", "nueva rama para", "start working on", or
  "voy a trabajar en X". Always confirm the branch name before creating it.
---

# Smart Branch

Generate a clean, conventional branch name, confirm with the user, then create and switch to it.

## Step 1: Understand what the branch is for

If the user already described what they're going to work on, extract that intent.
If it's not clear, ask one simple question: "¿Para qué es la rama?"

## Step 2: Generate the branch name

### Format
```
type/short-description-in-kebab-case
```

### Types — same as conventional commits
| Type | When to use |
|------|-------------|
| `feat` | New feature or behavior |
| `fix` | Bug fix |
| `hotfix` | Urgent fix that goes directly to production |
| `refactor` | Code restructure with no feature or bug impact |
| `chore` | Maintenance, config, dependencies |
| `ci` | CI/CD pipeline changes |
| `docs` | Documentation only |
| `test` | Adding or fixing tests |
| `perf` | Performance improvement |
| `build` | Build system or tooling |

### Description rules
- kebab-case, all lowercase
- Short and meaningful: 2–5 words max
- No articles (a, the, el, la) — go straight to the noun/verb
- Imperative or noun form: `add-login`, `fix-session-expiry`, `checkout-flow`
- No special characters, no slashes beyond the prefix

Good examples:
```
feat/add-guest-checkout
fix/auth-session-expiry
hotfix/payment-timeout
refactor/voucher-screen
chore/update-dependencies
ci/add-deploy-workflow
```

## Step 3: Confirm before creating

Show the branch name clearly and wait for confirmation:

```
Branch name:

  feat/add-guest-checkout

Should I create it and switch to it?
```

Never run `git checkout -b` before the user confirms.

## Step 4: Create and switch

Once confirmed:

```bash
git checkout -b feat/add-guest-checkout
```

Show confirmation: `Switched to branch 'feat/add-guest-checkout'`.

## Edge cases

- **User gives a vague description** (e.g., "una rama para el bug"): ask what the bug is about before generating.
- **Name would be too long**: trim it — the type prefix + 3–4 words is enough.
- **User already has a name in mind**: refine it to match the convention rather than replacing it entirely.
- **Already on a non-main branch**: mention it and ask if they still want to create a new branch from the current one or from `main`.
