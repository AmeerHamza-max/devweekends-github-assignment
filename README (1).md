# GIT & GITHUB

**Complete Practical Assignment Guide**

Step-by-step hands-on lab: from git init to a fully working, assignment-ready GitHub repository with a live GitHub Pages site.

*Prepared for University Submission*

## How to Use This Guide

This document is a complete practical lab manual. Follow the steps in exact order. Every command is given exactly as you should type it in your terminal (Git Bash, VS Code terminal, or any terminal with Git installed). Replace anything written in ALL_CAPS_WITH_UNDERSCORES (like YOUR_NAME, YOUR_REPO) with your own details.

> **Note:** Make sure Git is installed (check with: `git --version`) and you have a free GitHub account before starting.

### What You Will Build By The End

- A local Git repository with multiple meaningful commits
- Two branches, with practice switching between them
- That repository pushed to GitHub
- A Pull Request created and merged on GitHub
- A real merge conflict created on purpose, and resolved correctly
- A live website hosted using GitHub Pages
- A clean, well-organized, assignment-ready repository

---

## STEP 1: Create a Git Repository

A Git repository (repo) is a folder that Git tracks for version control. Every change inside this folder can be recorded as a "commit".

### 1.1 Create a project folder

```bash
mkdir git-assignment
cd git-assignment
```

### 1.2 Initialize Git

```bash
git init
```

This creates a hidden `.git` folder. That folder is the actual database where Git stores all history. Your project folder is now a Git repository.

### 1.3 Configure your identity (only needed once per machine)

```bash
git config --global user.name "YOUR_NAME"
git config --global user.email "YOUR_EMAIL@example.com"
```

### 1.4 Create your first file

```bash
echo "# Git Assignment" > README.md
```

### 1.5 Check repository status

```bash
git status
```

Git will show README.md as an "untracked file" — Git sees it but is not yet tracking changes to it.

---

## STEP 2: Make Multiple Commits

A commit is a saved snapshot of your project at a point in time. Professional projects always have many small, meaningful commits instead of one giant commit.

**STEP 1: Commit the README file**

```bash
git add README.md
git commit -m "Initial commit: add README"
```

**STEP 2: Add a second file and commit**

```bash
echo "console.log('Hello Git');" > app.js
git add app.js
git commit -m "Add app.js with hello world script"
```

**STEP 3: Modify a file and commit again**

```bash
echo "## Project Description" >> README.md
git add README.md
git commit -m "Update README with project description"
```

**STEP 4: Add a third file and commit**

```bash
echo "node_modules/" > .gitignore
git add .gitignore
git commit -m "Add .gitignore file"
```

### 2.5 View your commit history

```bash
git log --oneline
```

You should now see at least 4 commits listed, newest at the top. Each commit has a unique short hash (like `a4f3c2d`), the message you wrote, and represents a checkpoint you can return to.

> **Note:** Good commit messages are short, start with a capital letter, and describe WHAT changed (e.g. "Add login validation", not "fixed stuff").

---

## STEP 3: Create Two Branches

A branch is an independent line of development. The default branch is usually called "main". Branches let you work on new features without affecting the main code.

### 3.1 Check your current branch

```bash
git branch
```

### 3.2 Create the first branch: feature-navbar

```bash
git branch feature-navbar
```

### 3.3 Create the second branch: feature-footer

```bash
git branch feature-footer
```

### 3.4 List all branches

```bash
git branch
```

You should now see three branches: main, feature-navbar, and feature-footer. The branch with the `*` next to it is your current active branch.

---

## STEP 4: Branch Switching

### 4.1 Switch to feature-navbar

```bash
git switch feature-navbar
```

(Older command, still works: `git checkout feature-navbar`)

### 4.2 Make a change and commit it on this branch

```bash
echo "<nav>Home | About | Contact</nav>" > navbar.html
git add navbar.html
git commit -m "Add navbar component"
```

### 4.3 Switch to feature-footer

```bash
git switch feature-footer
```

### 4.4 Make a change and commit it on this branch too

```bash
echo "<footer>© 2026 My Project</footer>" > footer.html
git add footer.html
git commit -m "Add footer component"
```

### 4.5 Switch back to main

```bash
git switch main
```

Notice that navbar.html and footer.html do NOT appear in main yet — they only exist on their own branches until merged.

---

## STEP 5: Push the Repository to GitHub

### 5.1 Create a new repository on GitHub

- Go to github.com and log in
- Click the "+" icon (top right) → New repository
- Name it: `git-assignment`
- Leave it EMPTY — do NOT add README, .gitignore, or license (your local repo already has these)
- Click "Create repository"

### 5.2 Connect your local repo to GitHub

GitHub will show you a remote URL after creating the repo. Copy it and run:

```bash
git remote add origin https://github.com/YOUR_USERNAME/git-assignment.git
git remote -v
```

### 5.3 Push the main branch

```bash
git branch -M main
git push -u origin main
```

### 5.4 Push your other branches too

```bash
git push -u origin feature-navbar
git push -u origin feature-footer
```

Refresh your GitHub repository page — you should now see all three branches with their commits.

---

## STEP 6: Create a Pull Request (PR)

A Pull Request is a request to merge changes from one branch into another. It is reviewed before merging — this is how real teams collaborate safely.

### 6.1 Open the Pull Request page

- Go to your repository on GitHub
- Click the "Pull requests" tab
- Click "New pull request"
- Set base branch = main, compare branch = feature-navbar
- Click "Create pull request"
- Add a title: "Add navbar component" and a short description
- Click "Create pull request" again to confirm

### 6.2 Review the PR

GitHub shows you a "diff" — a side-by-side comparison of what changed. You (or a teammate) can add comments before approving.

---

## STEP 7: Merge the Pull Request

### 7.1 Merge on GitHub (recommended for this assignment)

- On the open Pull Request page, click "Merge pull request"
- Click "Confirm merge"
- Optionally click "Delete branch" to clean up feature-navbar on GitHub

### 7.2 Sync your local main branch

```bash
git switch main
git pull origin main
```

Your local main branch now contains navbar.html as well, because the PR merge happened on GitHub and you pulled it down.

### 7.3 (Alternative) Merging locally instead of on GitHub

```bash
git switch main
git merge feature-footer
git push origin main
```

This merges feature-footer locally and pushes the result. Either method (GitHub PR merge, or local git merge) is valid — for this assignment, do the navbar merge via PR on GitHub, and the footer merge locally, so you practice both.

---

## STEP 8: Create and Resolve a Merge Conflict (On Purpose)

A merge conflict happens when two branches change the SAME line of the SAME file differently, and Git cannot decide which version is correct. This is one of the most important Git skills to practice.

### 8.1 Create the conflict

On main branch, edit README.md:

```bash
git switch main
echo "Welcome message: Built with Git by Team A" > conflict.txt
git add conflict.txt
git commit -m "Add conflict.txt from main"
```

Now create a new branch from this point and change the SAME file differently:

```bash
git switch -c conflict-branch
echo "Welcome message: Built with Git by Team B" > conflict.txt
git add conflict.txt
git commit -m "Update conflict.txt from conflict-branch"
```

Now go back to main and change conflict.txt there too, on the same line:

```bash
git switch main
echo "Welcome message: Built with Git by Team Main" > conflict.txt
git add conflict.txt
git commit -m "Update conflict.txt from main"
```

### 8.2 Trigger the conflict

```bash
git merge conflict-branch
```

Git will stop and show:

```
CONFLICT (content): Merge conflict in conflict.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### 8.3 Open conflict.txt — Git marks the conflicting parts

```
<<<<<<< HEAD
Welcome message: Built with Git by Team Main
=======
Welcome message: Built with Git by Team B
>>>>>>> conflict-branch
```

HEAD shows the version on your current branch (main). The part after `=======` shows the version from the branch you are merging in (conflict-branch).

### 8.4 Resolve the conflict

Manually edit the file: delete the `<<<<<<<`, `=======`, `>>>>>>>` markers, and decide the final correct content. For example, you may keep one version, combine both, or write something new:

```
Welcome message: Built with Git by Team Main and Team B
```

### 8.5 Mark it resolved and commit

```bash
git add conflict.txt
git commit -m "Resolve merge conflict in conflict.txt"
git push origin main
```

> **Note:** `git status` before committing will show conflict.txt under "Unmerged paths" — always check this to be sure every conflict marker is removed before committing.

---

## STEP 9: Host a Website Using GitHub Pages

GitHub Pages lets you host a free static website directly from your GitHub repository.

### 9.1 Create a simple website file

```bash
git switch main
cat > index.html <<EOF
<!DOCTYPE html>
<html>
<head><title>My Git Assignment</title></head>
<body>
  <h1>Hello from GitHub Pages</h1>
  <p>This site proves my GitHub Pages setup works.</p>
</body>
</html>
EOF
```

(If the heredoc syntax above doesn't work in your terminal, simply create index.html manually in a text editor with that HTML content.)

### 9.2 Commit and push it

```bash
git add index.html
git commit -m "Add index.html for GitHub Pages"
git push origin main
```

### 9.3 Enable GitHub Pages

- Go to your repository on GitHub
- Click "Settings"
- In the left sidebar, click "Pages"
- Under "Build and deployment" → Source, select "Deploy from a branch"
- Branch: select "main" and folder "/ (root)"
- Click "Save"

### 9.4 View your live site

After 1-2 minutes, GitHub will show a URL in the same Pages settings, in this format:

```
https://YOUR_USERNAME.github.io/git-assignment/
```

Open that link in your browser. You should see your "Hello from GitHub Pages" page live on the internet.

---

## STEP 10: Make the Final Repository Assignment-Ready

### 10.1 Clean up unwanted files

```bash
git status
git clean -n
git clean -f
```

`git clean -n` shows what WOULD be deleted (dry run). `git clean -f` actually deletes untracked files. Use carefully.

### 10.2 Write a clear, complete README.md

Your final README.md should include:

- Project title
- Short description of the project
- List of branches used and what each one was for
- Steps you followed (init → commits → branches → merge → conflict resolution → GitHub Pages)
- Live GitHub Pages link
- Your name and roll number / class details (as required by your instructor)

### 10.3 Final check commands

```bash
git log --oneline --graph --all
git branch -a
git status
```

`git log --oneline --graph --all` gives you a visual tree of all commits and branches — a great screenshot to include in your assignment submission.

### 10.4 Final push

```bash
git add .
git commit -m "Final cleanup and documentation for submission"
git push origin main
```

### 10.5 Submission checklist

| Requirement | Status |
| --- | --- |
| Repository initialized with `git init` | Done in Step 1 |
| At least 4 meaningful commits | Done in Step 2 |
| Two branches created | Done in Step 3 |
| Switched between branches successfully | Done in Step 4 |
| Repository pushed to GitHub | Done in Step 5 |
| Pull Request created | Done in Step 6 |
| Pull Request merged | Done in Step 7 |
| Merge conflict created and resolved | Done in Step 8 |
| Website live via GitHub Pages | Done in Step 9 |
| README.md and repo cleaned up | Done in Step 10 |

**Once every row above is checked off, your repository is fully assignment-ready and 100% working.**
