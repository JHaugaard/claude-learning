# Fix: Separating Git Repositories from iCloud Sync

## Problem
Git repositories cloned into iCloud-synced folders can become corrupted when `.git` directories are synced between machines during concurrent git operations or mid-operation switching.

## Solution: Move `.git` Outside iCloud (Option A)

This guide moves the `.git` directory to a non-synced location on your machine and uses a symlink to keep git functionality working.

---

## Prerequisites
- A corrupted or at-risk git repository in an iCloud folder
- Terminal access
- Basic familiarity with `git` commands

---

## Step-by-Step Instructions

### Step 1: Recover the Repository (if corrupted)

If your repo shows git corruption errors (like "Could not read" or "bad revision"), start fresh:

```bash
# Navigate to your project directory
cd ~/Documents/dev-top/proposal_tracker_2

# Create a backup of current state
cp -r . ~/Documents/dev-top/proposal_tracker_2-backup

# Remove the corrupted .git
rm -rf .git

# Clone fresh from GitHub
git clone <your-repo-url> temp-clone

# Copy the fresh .git to your working directory
cp -r temp-clone/.git .

# Clean up
rm -rf temp-clone
```

### Step 2: Create Non-iCloud Storage for `.git`

Create a directory outside iCloud to store your `.git` directories:

```bash
# Create directory structure
mkdir -p ~/.local/git-storage

# Verify it's not synced to iCloud
ls -la ~/.local
# Should show .local is in your home directory, NOT in ~/Documents
```

### Step 3: Move `.git` Directory

For each repository you want to fix:

```bash
# Navigate to your project
cd ~/Documents/dev-top/proposal_tracker_2

# Create a unique storage location for this repo's .git
# (Use a descriptive name matching your project)
mkdir -p ~/.local/git-storage/proposal_tracker_2

# Move .git to the safe location
mv .git ~/.local/git-storage/proposal_tracker_2/
```

### Step 4: Create a Symlink

Create a symlink so git can still find the `.git` directory:

```bash
# From your project directory
cd ~/Documents/dev-top/proposal_tracker_2

# Create the symlink
ln -s ~/.local/git-storage/proposal_tracker_2/.git .git

# Verify the symlink works
ls -la | grep "\.git"
# Should show: .git -> ~/.local/git-storage/proposal_tracker_2/.git
```

### Step 5: Verify Everything Works

```bash
# Test basic git commands
git status
git log --oneline -5
git remote -v

# All should work without errors
```

### Step 6: Test Push/Pull

```bash
# Verify connection to remote
git fetch origin

# Make a small test change to confirm workflow
echo "# Test" >> README.md
git add README.md
git commit -m "Test commit after git fix"
git push origin development

# Then pull on another machine to verify
git pull origin development
```

---

## Applying to Other Projects

Repeat Steps 2-6 for each affected repository. Modify the paths and names accordingly:

```bash
# For another project:
mkdir -p ~/.local/git-storage/another_project_name
mv /path/to/other/project/.git ~/.local/git-storage/another_project_name/
cd /path/to/other/project
ln -s ~/.local/git-storage/another_project_name/.git .git
```

---

## Verification Checklist

After completing the fix:

- [ ] `git status` runs without errors
- [ ] `git log` displays commit history
- [ ] `git remote -v` shows correct GitHub URL
- [ ] `git push` successfully pushes to GitHub
- [ ] `git pull` successfully pulls from GitHub
- [ ] `.git` symlink is correctly pointing to `~/.local/git-storage/`
- [ ] Original working directory is still in iCloud (source files only)

---

## Future Prevention

1. **Only push before switching machines** — don't rely on iCloud sync of `.git`
2. **Verify symlinks persist** — After iCloud syncs, confirm symlinks still work: `ls -l .git`
3. **Consider new repos** — Clone new projects to `~/Dev` (non-synced) or ensure `.git` is symlinked immediately after cloning

---

## Troubleshooting

**Symlink disappeared after iCloud sync:**
```bash
# Recreate it
cd ~/Documents/dev-top/proposal_tracker_2
ln -s ~/.local/git-storage/proposal_tracker_2/.git .git
```

**Permission denied errors:**
```bash
# Ensure permissions are correct
chmod 755 ~/.local/git-storage
ls -la ~/.local/git-storage/
```

**Git still shows corruption:**
```bash
# Restart from Step 1 with a fresh clone from GitHub
```

---

## References

- [Git Documentation: Internals](https://git-scm.com/book/en/v2/Git-Internals)
- [Symlinks on macOS](https://support.apple.com/guide/mac-help/create-symbolic-links-in-terminal-mac-mchlp1017)
