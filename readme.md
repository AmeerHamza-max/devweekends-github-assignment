# Git — Complete Reference Guide

> **What is Git?**
> Git is a **distributed version control system (VCS)** — a tool that tracks and manages changes to files over time. Multiple people can collaborate on the same codebase simultaneously, with a full history of every change ever made.

---

## Table of Contents

1. [What is Version Control (VCS)?](#what-is-vcs)
2. [Installation & Setup](#installation--setup)
3. [Git Configuration](#git-configuration)
4. [Creating Your First Repository](#creating-your-first-repository)
5. [Git Workflow](#git-workflow)
6. [Git Commit](#git-commit)
7. [Viewing History — Git Log](#viewing-history--git-log)
8. [Viewing Old Versions](#viewing-old-versions)
9. [Git Restore — Undoing Changes](#git-restore--undoing-changes)
10. [Remote Repositories](#remote-repositories)
11. [Git Pull & Clone](#git-pull--clone)
12. [Branching & Merging](#branching--merging)
13. [Merge Conflicts](#merge-conflicts)
14. [Forking & Pull Requests](#forking--pull-requests)
15. [.gitignore](#gitignore)
16. [Git Clean](#git-clean)
17. [Git Tags](#git-tags)
18. [Hosting a Website on GitHub Pages](#hosting-a-website-on-github-pages)
19. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## What is VCS?

A **Version Control System** records every change made to your files — who made the change, when, and what was changed.

### Why use VCS?

| Benefit | Description |
|---|---|
| **Backup & Restore** | Files are safe against accidental loss or mistakes |
| **Collaboration** | Multiple people can work on the same project simultaneously |
| **Branching & Merging** | Experiment in isolation, then merge changes back safely |
| **Change Tracking** | See exactly what changed, when, and by whom |

---

## Installation & Setup

### Install Git on Windows

Download from [https://git-scm.com](https://git-scm.com) and run the installer.

Verify installation:

```bash
git --version
# Output: git version 2.x.x
```

### Basic Bash Commands (Git Bash)

```bash
mkdir myfolder        # Create a new folder
cd myfolder           # Navigate into the folder
touch index.html      # Create a new empty file
ls                    # List files in current folder
ls -la                # List ALL files (including hidden like .git)
```

---

## Git Configuration

Before using Git, tell it who you are. This info is attached to every commit you make.

```bash
git config --global user.name "AmeerHamza-max"
git config --global user.email "ameerhamzarana0000787@gmail.com"

# Verify your config
git config --list
```

> `--global` means this applies to ALL projects on your machine. Use `--local` to set config for one project only.

---

## Creating Your First Repository

A **repository (repo)** is a folder that Git tracks. To start tracking a folder:

```bash
git init
```

This creates a hidden `.git` folder inside your project:

```bash
ls -la
# You'll see: .git/
```

> The `.git` folder is the "brain" of your repo — it stores ALL history, commits, and metadata. **Never delete it manually.**

---

## Git Workflow

```
┌─────────────────────────────────────────────────────────┐
│                    GIT WORKFLOW                          │
│                                                         │
│  Working Directory  →  Staging Area  →  Local Repo      │
│  (your files)          (git add)        (git commit)    │
│                                                         │
│        ←──────────── git checkout ──────────────        │
└─────────────────────────────────────────────────────────┘
```

| Zone | Description |
|---|---|
| **Working Directory** | Where you write and edit code |
| **Staging Area** | Files marked to be included in the next commit |
| **Local Repository** | Permanent snapshot history stored in `.git` |

---

## Git Commit

### Step 1 — Check Status

```bash
git status
# Shows: modified files, new files, staged files
```

### Step 2 — Stage Files

```bash
git add index.html         # Stage a single file
git add style.css          # Stage another file
git add .                  # Stage ALL changed files at once
```

> Staging is important! Only staged files are included in the next commit.

### Step 3 — Commit

```bash
git commit -m "First version of index.html"
```

> A **commit** is a permanent snapshot of your staged changes. It gets a unique ID (hash), your name, email, timestamp, and message.

### Example — Full Cycle

```bash
# Make changes to index.html, then:
git status                        # See what changed
git diff                          # See exact line-by-line changes
git add index.html
git commit -m "Added navbar - v2"
```

---

## Viewing History — Git Log

### Basic Log

```bash
git log
# Shows: commit ID, author, date, message
```

### Useful Log Options

```bash
git log -p -2                        # Show last 2 commits WITH file diffs
git log --stat                       # Summary of files changed per commit
git log --pretty=oneline             # One line per commit
git log --pretty=format:"%h - %an, %ar : %s"
# %h = short hash | %an = author | %ar = relative time | %s = subject
```

### Search Through Log

```bash
git log -S "h1"                      # Commits that added/removed the string "h1"
git log -S "V2"                      # Search for specific version string
git log --grep='version'             # Commits whose message contains 'version'
git log --grep='fix bugs'
```

### Filter by Date / Author

```bash
git log --since='2026-06-22'         # Commits after this date
git log --until='2026-05-11'         # Commits before this date
git log --author='AmeerHamza-max'    # Only this author's commits
git log --no-merges                  # Exclude merge commits
```

---

## Viewing Old Versions

### View a Specific File at an Old Commit

```bash
git show <commit-id>:<file-path>

# Example:
git show 08bf1fe686d6a8813ee54b622a80c72e74f60601:index.html
```

> Copy the full commit ID from `git log`.

### Restore Old Files to Working Directory (Git Checkout)

When you want to actually open/edit old files in VS Code:

```bash
git checkout <commit-id> -- index.html     # Restore one file
git checkout <commit-id> -- *              # Restore ALL files

# Example:
git checkout 97f599153f04b857f6a97eb02f1df47232eff8de -- *
```

### Return to Latest Version

```bash
git checkout master -- *      # Restore everything to latest commit
git status                    # Confirm you're back to latest
```

---

## Git Restore — Undoing Changes

### Case 1 — Undo Unsaved Changes (not staged yet)

```bash
git restore .                  # Undo ALL files
git restore index.html         # Undo a specific file
```

### Case 2 — Unstage a File (was added with `git add`)

```bash
git restore --staged index.html    # Remove from staging, keep changes
git restore --staged .             # Unstage all files

# Then to also discard the actual changes:
git restore .
```

### Case 3 — Discard Unstaged Changes (keep staged)

```bash
# You staged some changes, then made MORE changes after staging.
# To remove only the extra (unstaged) changes:
git restore --worktree index.html
```

### Case 4 — Undo a Commit (Reset)

```bash
git reset --soft HEAD^    # Undo last commit, KEEP changes (staged)
git reset --hard HEAD^    # Undo last commit, DISCARD all changes
```

> `HEAD^` means "one commit before current". Use `HEAD~2` for two commits back.

---

## Remote Repositories

A **remote repository** is a version of your project hosted on the internet (e.g., GitHub).

### Link Local Repo to GitHub

```bash
git remote add origin https://github.com/AmeerHamza-max/devweekends-github-assignment.git
```

### Set Main Branch and Push

```bash
git branch -M main                 # Rename branch to 'main'
git push -u origin main            # Push code to GitHub (first time)
git push                           # Push subsequent changes
```

### Verify Remote Connection

```bash
git remote          # Shows: origin
git remote -v       # Shows full URL
```

---

## Git Pull & Clone

### Git Pull — Sync Remote → Local

```bash
# Fetch + merge all changes from remote into your local branch
git pull
```

### Git Clone — Download an Entire Repo

```
┌─────────────────────────────────────────────────────┐
│               GIT CLONE WORKFLOW                    │
│                                                     │
│   GitHub Remote Repo                                │
│         │                                           │
│         │  git clone <url>                          │
│         ▼                                           │
│   Local Copy (full history included)                │
│         │                                           │
│         │  git pull  (get future updates)           │
│         ▼                                           │
│   Updated Local Copy                                │
└─────────────────────────────────────────────────────┘
```

```bash
git clone https://github.com/AmeerHamza-max/devweekends-github-assignment.git

cd devweekends-github-assignment    # Enter the cloned folder
git pull                            # Later, get any new changes
```

---

## Branching & Merging

Branches let different team members work in isolation — a designer works in `design`, a developer in `feature/login` — without affecting the `main` branch.

```
main:    A ─── B ─── C ─────────── M   (merge)
                      \           /
design:               D ─── E ─── F
```

### Branch Commands

```bash
git branch                     # List all branches
git branch design              # Create a new branch called 'design'
git checkout design            # Switch to 'design' branch
git checkout -b feature/login  # Create AND switch in one command
git checkout main              # Switch back to main
```

### Merging a Branch into Main

```bash
git checkout main              # Be on the branch you want to merge INTO
git merge design               # Bring design branch changes into main
```

### Push a Branch to GitHub

```bash
git add index.html
git commit -m "Changes in design branch"
git push -u origin design      # Push branch to GitHub
```

---

## Merge Conflicts

A **merge conflict** happens when two people changed the **same line** of the same file differently. Git can't decide which version to keep — you must resolve it manually.

```
<<<<<<< HEAD
  <h1>Welcome to our site</h1>       ← your current branch version
=======
  <h1>Hello from design branch</h1>  ← incoming branch version
>>>>>>> design
```

**Steps to resolve:**

1. Open the file in VS Code
2. Delete the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Keep the correct version (or combine both)
4. Save the file
5. `git add index.html`
6. `git commit -m "Resolved merge conflict in index.html"`

---

## Forking & Pull Requests

**Forking** = making your own copy of someone else's repo on GitHub. Used when you don't have direct write access.

### Workflow

```
Original Repo (someone else's)
       │
       │  Fork (on GitHub)
       ▼
Your GitHub Fork
       │
       │  git clone
       ▼
Your Local Copy
       │
       │  make changes → commit → push
       ▼
Your GitHub Fork
       │
       │  Pull Request (PR)
       ▼
Original Repo (owner reviews & merges)
```

```bash
# After forking on GitHub and cloning:
git pull                              # Get latest changes from your fork
git checkout -b my-feature            # Work on a new branch
# ... make changes ...
git add .
git commit -m "Added my feature"
git push origin my-feature
# Then open a Pull Request on GitHub
```

---

## .gitignore

A `.gitignore` file tells Git which files to **never track or upload** — like secrets, logs, build files.

Create a file named `.gitignore` in your repo root:

```
# .gitignore
app.log
secrets.json
*.log           # ignore ALL .log files
node_modules/   # ignore entire folder
.env            # ignore environment variables file
dist/           # ignore build output folder
```

> Files already committed won't be ignored until you remove them from tracking with `git rm --cached <file>`.

---

## Git Clean

Remove **untracked** files (files Git doesn't know about — never added or committed):

```bash
git clean -n          # Dry run — shows what WOULD be deleted
git clean -f          # Actually delete untracked files
git clean -fd         # Delete untracked files AND folders
```

> Always run `git clean -n` first to preview before deleting!

---

## Git Tags

Tags give meaningful names to specific commits — useful for marking release versions.

### Create a Tag

```bash
git tag -a V7 -m "this is version 7"          # Annotated tag on latest commit
git tag -a v1.0 -m "My version 1.0"           # Standard release tag
git tag -a v1.2 <commit-id>                    # Tag an OLD commit
```

### View Tags

```bash
git tag                # List all tags
git show V7            # Show details of a tag (commit info + message)
git show v1.0
```

### Delete a Tag

```bash
git tag -d v1.0                    # Delete local tag
git push origin --delete v1.0      # Delete tag from GitHub
```

### Push Tags to GitHub

```bash
git push origin v1.0               # Push one tag
git push origin --tags             # Push ALL tags
```

---

## Hosting a Website on GitHub Pages

GitHub Pages lets you host a static website **free** directly from your repository.

### Steps

1. Push your project to GitHub (must have `index.html` in root)
2. Go to your repo on GitHub
3. Click **Settings** → **Pages** (in the left sidebar)
4. Under **Source**, select branch `main` and folder `/ (root)`
5. Click **Save**
6. Your site will be live at:

```
https://<your-username>.github.io/<repo-name>/
```

> Example: `https://AmeerHamza-max.github.io/devweekends-github-assignment/`

---

## Quick Reference Cheat Sheet

### Setup
| Command | Description |
|---|---|
| `git config --global user.name "Name"` | Set username |
| `git config --global user.email "email"` | Set email |
| `git init` | Initialize a new repo |

### Daily Workflow
| Command | Description |
|---|---|
| `git status` | Check what's changed |
| `git diff` | See exact line changes |
| `git add .` | Stage all changes |
| `git add <file>` | Stage one file |
| `git commit -m "message"` | Commit with message |
| `git log --oneline` | Quick history view |

### Undoing
| Command | Description |
|---|---|
| `git restore .` | Discard all unstaged changes |
| `git restore --staged .` | Unstage all files |
| `git reset --soft HEAD^` | Undo commit, keep changes |
| `git reset --hard HEAD^` | Undo commit, delete changes |

### Remote
| Command | Description |
|---|---|
| `git remote add origin <url>` | Link to GitHub |
| `git push -u origin main` | First push |
| `git push` | Push changes |
| `git pull` | Get latest changes |
| `git clone <url>` | Download a repo |

### Branches
| Command | Description |
|---|---|
| `git branch` | List branches |
| `git branch <name>` | Create branch |
| `git checkout <name>` | Switch branch |
| `git checkout -b <name>` | Create + switch |
| `git merge <name>` | Merge into current branch |

### Tags
| Command | Description |
|---|---|
| `git tag -a v1.0 -m "msg"` | Create annotated tag |
| `git tag` | List all tags |
| `git show v1.0` | Show tag details |
| `git tag -d v1.0` | Delete local tag |
| `git push origin --tags` | Push all tags |

---

*Guide compiled from Dev Weekends Summer Fellowship '26 — Brave Packet Clan*
