Yes. **Branches are better** than both pushing directly to `main`.

You cannot guarantee **zero** merge conflicts, but branches make conflicts safer because you resolve them **before** they reach `main`.

# Is it normal for a branch to be behind `main`?

Yes. Completely normal.

Example:

```text
main:     A---B---C---D
               \
branch:         E---F
```

Your branch was created from commit `B`.

Then someone pushed `C` and `D` to `main`.

Now your branch is **behind main** by two commits.

That is not an error. It just means your branch has not yet included the latest changes from `main`.

# Best workflow for two people

Use this structure:

```text
main
├── person-a-feature
└── person-b-feature
```

Then merge into `main` through Pull Requests.

## Setup rule

`main` should be the clean, working version.

Nobody works directly on `main` unless it is a tiny emergency fix.

# Daily branch workflow

## 1. Start from updated main

```bash
git switch main
git pull --rebase origin main
```

## 2. Create your own branch

```bash
git switch -c feature/landing-page
```

Example branch names:

```text
feature/landing-page
feature/dashboard-alerts
fix/navbar-spacing
fix/websocket-connection
```

## 3. Work normally

```bash
git status
git add .
git commit -m "Add landing page hero section"
```

## 4. Push your branch

```bash
git push -u origin feature/landing-page
```

Then open a **Pull Request** on GitHub.

# How to keep your branch updated with main

While you are working, `main` may move ahead.

Use this:

```bash
git fetch origin
git switch feature/landing-page
git rebase origin/main
```

Then push your updated branch:

```bash
git push --force-with-lease
```

That makes your branch sit on top of the latest `main`.

Clean history:

```text
main:     A---B---C---D
                       \
branch:                 E---F
```

# If you get conflicts during rebase

Git will stop and show the conflicted files.

Check:

```bash
git status
```

Open the conflicted file. You may see:

```text
<<<<<<< HEAD
code from main
=======
your branch code
>>>>>>> feature/landing-page
```

Fix the file manually.

Then:

```bash
git add .
git rebase --continue
```

When done:

```bash
git push --force-with-lease
```

# Important rule about `rebase`

Use `rebase` freely on **your own branch**.

Do **not** rebase a branch that both of you are actively sharing unless you both understand what is happening.

For a shared branch, use merge instead:

```bash
git fetch origin
git switch shared-branch
git merge origin/main
git push
```

But the better structure is:

```text
Each person gets their own branch.
No shared work branch unless necessary.
```

# Best GitHub setup

On GitHub, protect `main`.

Recommended settings:

```text
Require pull request before merging
Require approvals: 1
Block direct pushes to main
Allow force pushes: off
Allow deletions: off
```

Then the workflow becomes:

```text
branch → pull request → review → merge into main
```

This prevents someone from accidentally breaking `main`.

# Best merge method

For your use case, use:

```text
Squash and merge
```

Why?

It keeps `main` clean.

Instead of this:

```text
Add button
Fix button
Fix button again
Actually fix button
```

`main` gets one clean commit:

```text
Add dashboard alert button
```

# Your golden workflow

## When starting new work

```bash
git switch main
git pull --rebase origin main
git switch -c feature/my-change
```

## While working

```bash
git add .
git commit -m "Clear message"
```

## Before opening PR

```bash
git fetch origin
git rebase origin/main
git push -u origin feature/my-change
```

## If the branch already exists remotely

```bash
git push --force-with-lease
```

## After PR is merged

```bash
git switch main
git pull --rebase origin main
git branch -d feature/my-change
```

Optional: delete remote branch too:

```bash
git push origin --delete feature/my-change
```

# How to avoid merge problems

You cannot eliminate them, but this reduces them heavily:

| Rule                                | Why it matters                    |
| ----------------------------------- | --------------------------------- |
| Use separate branches               | Main stays safe                   |
| Pull latest `main` before branching | Reduces stale work                |
| Rebase your branch often            | Keeps branch close to main        |
| Make small commits                  | Easier conflict resolution        |
| Make small PRs                      | Easier review and merging         |
| Avoid both editing same file        | Most conflicts happen here        |
| Do not force push to `main`         | Prevents history damage           |
| Protect `main`                      | Prevents accidental direct pushes |

# Recommended simple team rule

Use this agreement:

```text
Nobody pushes directly to main.
Each person works on their own branch.
Before making a PR, rebase onto latest main.
Merge using squash and merge.
Delete branch after merge.
```

That is the cleanest setup for two people.
