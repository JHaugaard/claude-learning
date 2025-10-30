# Git Essentials: 10 Most Used Commands

Quick reference for the commands you'll use daily. Print this out or keep it handy.

---

## 1. `git status`
**What:** Shows which files are modified, staged, or untracked.
**Why:** Understand what's changed before committing.
**When:** Multiple times per session — before staging, before committing, before pushing.

```bash
git status
```

---

## 2. `git add`
**What:** Stages files for commit (moves them to the staging area).
**Why:** Lets you choose exactly which changes go into your next commit.
**When:** After editing files, before you commit.

```bash
git add filename.ts          # Stage one file
git add .                    # Stage all changes
git add src/                 # Stage entire directory
```

---

## 3. `git commit`
**What:** Creates a snapshot of staged changes with a message.
**Why:** Saves your work with a description of what changed and why.
**When:** After staging, when a logical unit of work is complete.

```bash
git commit -m "Brief description of changes"
```

---

## 4. `git push`
**What:** Sends your local commits to the remote repository (GitHub).
**Why:** Backs up your work and shares it with collaborators.
**When:** At the end of a work session or when a feature is complete.

```bash
git push origin main               # Push to main branch
git push origin development        # Push to development branch
git push                           # Push current branch (if tracking)
```

---

## 5. `git pull`
**What:** Fetches changes from remote and merges them into your local branch.
**Why:** Keeps your code up-to-date with team changes.
**When:** At the start of a session or before making changes on a shared branch.

```bash
git pull origin main
git pull                     # Pull current tracked branch
```

---

## 6. `git log`
**What:** Shows the commit history.
**Why:** Review what changes were made, by whom, and when.
**When:** Debugging, understanding code history, or finding a specific commit.

```bash
git log --oneline -10        # Last 10 commits, condensed
git log --oneline            # Full history, one line per commit
git log -p filename.ts       # See changes to a specific file
```

---

## 7. `git branch`
**What:** Lists, creates, or deletes branches.
**Why:** Organize work without affecting main code.
**When:** Starting a new feature or fixing a bug.

```bash
git branch                   # List local branches
git branch feature-name      # Create new branch
git branch -d feature-name   # Delete branch
git checkout -b feature-name # Create and switch to new branch
```

---

## 8. `git checkout`
**What:** Switches between branches or restores files.
**Why:** Move to different lines of work or undo file changes.
**When:** Switching contexts, or reverting unwanted edits.

```bash
git checkout main                    # Switch to main branch
git checkout feature-name            # Switch to another branch
git checkout filename.ts             # Discard changes to a file
```

---

## 9. `git merge`
**What:** Combines commits from one branch into another.
**Why:** Integrate completed features back to main.
**When:** Feature branch is done; ready to merge into main/development.

```bash
git checkout main
git merge feature-name       # Merge feature into main
```

---

## 10. `git diff`
**What:** Shows differences between files or commits.
**Why:** Review exactly what changed before staging/committing.
**When:** Before staging, to catch unintended changes.

```bash
git diff                     # Changes in working directory
git diff --staged            # Changes staged for commit
git diff main feature-name   # Differences between branches
```

---

## Quick Workflow: A Typical Session

```bash
# Start
git pull origin development          # Get latest changes

# Work
git status                           # See what you've changed
git add .                            # Stage your changes
git commit -m "Add new feature"      # Commit with message

# Share
git push origin development          # Push to remote

# Next session
git pull origin development          # Repeat
```

---

## Quick Reference Card

| Command | Usage |
|---------|-------|
| `git status` | Check current state |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Save changes |
| `git push` | Send to GitHub |
| `git pull` | Get latest from GitHub |
| `git log -10` | View recent commits |
| `git branch` | List branches |
| `git checkout -b name` | Create & switch branch |
| `git merge branch` | Merge branch into current |
| `git diff` | See what changed |

---

## Key Principles

1. **Commit often** — small, logical commits are easier to manage
2. **Write clear messages** — "Fix bug" vs "Fix null pointer in authentication check"
3. **Push before switching machines** — don't rely on file sync
4. **Pull before pushing** — ensure you have latest changes
5. **Use branches** — keep main stable, experiment on feature branches
