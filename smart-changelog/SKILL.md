---
name: smart-changelog
description: >
  Generates or updates the CHANGELOG.md in the project root by reading commits
  since the last version tag or from a specific range. Groups changes by type
  following Keep a Changelog format. Use this skill whenever the user wants to
  update the changelog, generate release notes, or document what changed in a
  version. Triggers on "update changelog", "generate changelog", "actualizar
  changelog", "release notes", "what changed in this version".
---

# Smart Changelog

Read the commit history, group changes by type, and update CHANGELOG.md in the project root.

## Step 1: Understand the scope

Ask the user if you don't know:
- Is this for a specific version/tag? (e.g., `v1.3.0`)
- Or just "everything since the last release"?

Then run:

```bash
# Get the last tag
git describe --tags --abbrev=0

# Get commits since last tag
git log <last-tag>..HEAD --oneline --no-merges

# Or if no tags exist, get all commits
git log --oneline --no-merges
```

## Step 2: Group commits by type

Map conventional commit types to changelog sections:

| Commit type | Changelog section |
|-------------|------------------|
| `feat` | ### Features |
| `fix` | ### Bug Fixes |
| `perf` | ### Performance |
| `refactor` | ### Improvements |
| `docs` | ### Documentation |
| `build`, `ci`, `chore` | ### Maintenance |
| `BREAKING CHANGE` | ### Breaking Changes ⚠️ |

Ignore `style`, `test` commits — they don't belong in a user-facing changelog.

Skip merge commits entirely.

## Step 3: Write the changelog entry

### Format — Keep a Changelog standard

```markdown
## [1.3.0] - 2026-03-11

### Breaking Changes ⚠️
- **feat!(auth):** removed support for legacy token format — migrate to JWT

### Features
- **feat(checkout):** add guest checkout flow
- **feat(profile):** allow users to upload a profile picture

### Bug Fixes
- **fix(auth):** redirect to login when session expires
- **fix(cart):** prevent duplicate items when adding from search

### Improvements
- **refactor(payment):** simplify payment state management

### Maintenance
- **chore(deps):** bump react-navigation to v6.8.1
- **ci:** add automated deploy on merge to main
```

**Rules:**
- Version header: `## [x.y.z] - YYYY-MM-DD`
- If version isn't known yet, use `## [Unreleased]`
- Each entry: `- **type(scope):** description` — keep the conventional commit reference
- Descriptions in sentence case, no period at the end
- Breaking changes always go first

## Step 4: Update CHANGELOG.md

Check if CHANGELOG.md exists in the project root:

```bash
ls CHANGELOG.md
```

- **If it exists**: prepend the new entry at the top, below the title header
- **If it doesn't exist**: create it with a proper header first

```markdown
# Changelog

All notable changes to this project will be documented here.
Format based on [Keep a Changelog](https://keepachangelog.com).

## [1.3.0] - 2026-03-11
...
```

Show the user the new entry before writing:

```
Here's the changelog entry I'll add:

---
[entry here]
---

Should I add this to CHANGELOG.md?
```

Never write to CHANGELOG.md before confirmation.

## Step 5: Write the file

Once confirmed, update the file and show: `CHANGELOG.md updated with version x.y.z`.

## Edge cases

- **No tags exist**: Use all commits and suggest `## [Unreleased]` as the header
- **Commits don't follow conventional commits**: Do your best to categorize by reading the message intent, and flag the ones you're unsure about
- **Merge commits**: Skip them — they add noise
- **Many commits**: Focus on user-facing changes; condense multiple chore/ci commits into one line if they're repetitive
