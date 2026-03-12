---
name: smart-pr
description: >
  Generates a pull request title and description by analyzing all commits in the
  current branch compared to main. Shows what changed, why, and how to test it.
  Use this skill whenever the user wants to open a PR, create a pull request, or
  push their branch for review. Triggers on "quiero abrir un PR", "create a PR",
  "open pull request", "subir para review", "hacer el PR". Always confirm the
  description before creating the PR.
---

# Smart PR

Analyze the branch commits, generate a clear PR description, confirm with the user, then open the PR.

## Step 1: Understand what changed in this branch

```bash
git log main..HEAD --oneline
git diff main..HEAD --stat
```

Read the commits to understand:
- What features or fixes were added
- Which files and modules were touched
- Whether there are breaking changes

## Step 2: Generate the PR title and description

### Title
- Follow the same conventional commits format: `type(scope): short description`
- If the branch has multiple commits of different types, pick the most significant one
- Imperative mood, lowercase, no period

### Description — use this structure

```markdown
## What changed
<!-- Brief summary of what this PR does -->

## Why
<!-- The reason or motivation behind this change -->

## Changes
<!-- List the key changes grouped by area -->

## How to test
<!-- Steps to verify the changes work correctly -->
```

**Rules:**
- Keep it concise — the diff tells the full story
- "What changed" and "Why" should be 1-3 sentences max
- "Changes" should be a bullet list grouped by module if there are multiple areas
- "How to test" should be actionable steps, not vague instructions
- Use **fake/generic examples** — never expose real internal data in the template

**Example output:**
```markdown
## What changed
Added support for guest checkout flow, allowing users to complete a purchase without creating an account.

## Why
A significant portion of users were dropping off at the sign-up step during checkout.

## Changes
- `feat(checkout)`: add guest checkout option on the cart screen
- `feat(auth)`: allow unauthenticated users to reach the payment step
- `fix(order)`: handle missing user ID in order creation for guests

## How to test
1. Open the app and add any item to your cart
2. Tap "Checkout" without logging in
3. Verify the guest checkout option appears
4. Complete a test purchase and confirm the order is created correctly
```

## Step 3: Confirm before creating

Show the full title and description and wait:

```
Here's the PR I'll open:

Title: feat(checkout): add guest checkout flow

---
[description here]
---

Look good? I'll create it once you confirm.
(You can ask me to adjust anything before I do.)
```

Never run `gh pr create` before explicit confirmation.

## Step 4: Create the PR

Once confirmed:

```bash
gh pr create --title "type(scope): description" --body "$(cat <<'EOF'
## What changed
...

## Why
...

## Changes
...

## How to test
...
EOF
)"
```

Show the PR URL after creation.

## Edge cases

- **Only one commit**: The PR description can be shorter — just "What changed" and "How to test".
- **Many commits across unrelated areas**: Group changes by module in the "Changes" section.
- **Branch is behind main**: Warn the user and suggest rebasing before opening the PR.
- **No commits ahead of main**: Tell the user there's nothing to PR yet.
