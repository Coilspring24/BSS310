You **cannot fully guarantee no conflicts** if both of you push directly to `main`.

But you can reduce conflicts massively by using the correct Git flow:

```text
Always update your local main before you start working.
Commit your own work locally.
Pull/rebase the latest main before pushing.
Resolve conflicts locally if needed.
Then push.
```

# Best direct-to-main workflow

Use this every time before you work:

```bash
git switch main
git pull --rebase origin main
```

Then do your work.

Check what changed:

```bash
git status
```

Stage and commit:

```bash
git add .
git commit -m "Describe what you changed"
```

Before pushing, update again:

```bash
git pull --rebase origin main
```

Then push:

```bash
git push origin main
```

That is the basic safe loop.

# The full daily workflow

## 1. Start work

```bash
git switch main
git pull --rebase origin main
```

## 2. Edit files

Make your changes.

## 3. Check changes

```bash
git status
git diff
```

## 4. Commit your work

```bash
git add .
git commit -m "Add landing page hero section"
```

## 5. Re-sync before push

```bash
git pull --rebase origin main
```

## 6. Push

```bash
git push origin main
```

# Why `pull --rebase`?

Avoid this as your normal workflow:

```bash
git pull origin main
```

That can create messy merge commits like:

```text
Merge branch 'main' of github.com:...
```

Prefer:

```bash
git pull --rebase origin main
```

This takes your local commits, updates `main`, then replays your commits on top.

Cleaner history:

```text
A---B---C---your commit
```

Instead of:

```text
A---B---C
     \   \
      D---Merge commit
```

# Set this permanently

Run this once:

```bash
git config --global pull.rebase true
git config --global rebase.autoStash true
```

Now this:

```bash
git pull
```

will behave like:

```bash
git pull --rebase
```

And if you have uncommitted changes, Git will try to temporarily stash them during the rebase.

# If Git says your branch is behind

Example:

```text
Updates were rejected because the remote contains work that you do not have locally.
```

Do:

```bash
git pull --rebase origin main
git push origin main
```

# If there is a conflict

Git may show something like:

```text
CONFLICT (content): Merge conflict in index.html
```

Open the file. You will see:

```text
<<<<<<< HEAD
your version
=======
their version
>>>>>>> main
```

Edit the file manually so it contains the correct final version.

Then:

```bash
git add index.html
git rebase --continue
```

After the rebase finishes:

```bash
git push origin main
```

# If you panic during a rebase

Abort and return to how things were:

```bash
git rebase --abort
```

Then ask what happened before doing anything destructive.

# Rules you and your friend should follow

## Rule 1: Pull before editing

Before starting:

```bash
git pull --rebase origin main
```

## Rule 2: Pull before pushing

Before pushing:

```bash
git pull --rebase origin main
```

## Rule 3: Do not both edit the same file at the same time

Conflicts usually happen when both people edit the same lines in the same file.

Example danger files:

```text
index.html
server.js
README.md
package.json
```

Split work clearly:

```text
Person A: frontend / public/index.html
Person B: backend / server.js
```

Or:

```text
Person A: landing page
Person B: dashboard
```

## Rule 4: Commit small changes often

Bad:

```bash
git commit -m "changed everything"
```

Good:

```bash
git commit -m "Add dashboard status cards"
git commit -m "Update websocket alert handler"
git commit -m "Fix mobile navbar spacing"
```

Small commits are easier to rebase and fix.

## Rule 5: Never use force push on main

Avoid:

```bash
git push --force
```

Especially on `main`.

If you absolutely need it, use this safer version:

```bash
git push --force-with-lease
```

But for your current setup: **do not force push to main**.

# Recommended safer workflow

Even better: do not both push directly to `main`.

Use branches:

```bash
git switch main
git pull --rebase origin main
git switch -c landing-page-update
```

Work, commit, push:

```bash
git add .
git commit -m "Update landing page"
git push -u origin landing-page-update
```

Then open a Pull Request on GitHub.

This is much safer because GitHub shows conflicts before merging.

# Minimal command cheat sheet

## Before work

```bash
git switch main
git pull --rebase origin main
```

## Save work

```bash
git status
git add .
git commit -m "Message"
```

## Before push

```bash
git pull --rebase origin main
git push origin main
```

## Conflict resolved

```bash
git add .
git rebase --continue
git push origin main
```

## Abort rebase

```bash
git rebase --abort
```

# Best setup for your case

Since you are both working on `main`, use this as your golden rule:

```bash
git pull --rebase origin main
# work
git add .
git commit -m "Clear message"
git pull --rebase origin main
git push origin main
```

That is the cleanest direct-to-main Git operation flow.
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
