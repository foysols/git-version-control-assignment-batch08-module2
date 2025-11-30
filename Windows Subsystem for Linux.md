# Windows Subsystem for Linux

Practical steps to enable WSL, set up Ubuntu, and use common Git/GitHub workflows.

## Enable WSL (PowerShell as Administrator)
```powershell
wsl --install
wsl --set-default-version 2
```

## Install Ubuntu 24.04
```powershell
wsl --list --online
wsl --install -d Ubuntu-24.04
```

## Enter WSL & Update Packages
```bash
wsl
sudo apt update && sudo apt upgrade -y
```

## Backup, Restore, or Remove a Distro
```powershell
# Backup
wsl --export Ubuntu-24.04 "C:\WSL-Backup\Ubuntu-Backup.tar"
# Restore
wsl --import Ubuntu-24.04 "." "C:\WSL-Backup\Ubuntu-Backup.tar"
# Delete corrupted instance
wsl --list --verbose
wsl --unregister Ubuntu-24.04
```

## Install Git on Ubuntu
```bash
sudo apt install git -y
git --version
```

## GitHub CLI Setup (Ubuntu)
```bash
sudo apt install curl -y
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh -y
gh --version
gh auth login
```

## SSH Setup & Clone (private repos)
```bash
ssh-keygen -t ed25519 -C "md-sarowar-alam"
cat /.ssh/id_ed25519.pub   # add this key to GitHub: https://github.com/settings/keys
ssh -T git@github.com       # confirm: "Hi <user>! You've successfully authenticated..."
git clone git@github.com:md-sarowar-alam/ostad-class-08.git
```

## Day-to-day Git Workflow
```bash
# Create a branch (avoid working directly on main)
git checkout -b feature/my-branch

# Edit files, then stage and commit
git status
git add .
git commit -m "My updates in branch"

# Push branch
git push -u origin feature/my-branch

# Get latest updates for the current branch
git pull
```

### Sync branch with main (merge)
```bash
git checkout feature/my-branch
git fetch origin
git merge origin/main
```

### Sync branch with main (rebase)
```bash
git checkout feature/my-branch
git fetch origin
git rebase origin/main
```

### Hard refresh branch to match main (destructive)
```bash
git checkout feature/my-branch
git fetch origin
git reset --hard origin/main
git push --force origin feature/my-branch
```

### Delete a branch
```bash
git checkout main
git branch -D feature/my-branch             # local
git push origin --delete feature/my-branch  # remote
```

## Merge a branch into main
```bash
git checkout main
git pull origin main
git merge feature/my-branch
git push origin main
```

## Inspect history
```bash
git log --oneline --graph --decorate --all
```

## Branching examples (from branch or main)
```bash
# From feature/my-branch
git checkout feature/my-branch
git checkout -b feature/my-branch-from-branch

# From main
git checkout main
git checkout -b feature/my-branch-from-main
```

### Merge across branches
```bash
# Merge feature/my-branch-from-main into feature/my-branch-from-branch
git checkout feature/my-branch-from-branch
git fetch origin
git merge origin/feature/my-branch-from-main
git push origin feature/my-branch-from-branch

# Merge feature/my-branch-from-branch into feature/my-branch-from-main
git checkout feature/my-branch-from-main
git fetch origin
git merge origin/feature/my-branch-from-branch
git push origin feature/my-branch-from-main
```

### Resolve conflicts
```bash
vi README.md            # resolve
git add .
git commit -m "Resolve merge conflicts"
git push origin feature/my-branch-from-branch
```

## Create a local repo and push to GitHub
```bash
mkdir my-local-repo-creation-private
cd my-local-repo-creation-private
git init
echo "Hello World" > readme.txt
git add .
git commit -m "Creating repo from Ubuntu"
gh repo create my-local-repo-creation-private --private --source=. --remote=origin --push
```

## Quick Git examples
```bash
# Clone specific branch
git clone --branch feature/test-change-02 --single-branch git@github.com:md-sarowar-alam/batch-08-class-git.git

# Merge main into branch
git checkout feature/test-change-02
git pull origin main

# Rebase branch onto main
git fetch origin
git rebase origin/main

# Reset branch to main
git checkout feature/my-change
git fetch origin
git reset --hard origin/main
git push --force origin feature/my-change

# Create a new branch
git checkout -b feature/edit-readme
```

## Clone, stage, commit, push (essentials)
```bash
git clone git@github.com:md-sarowar-alam/batch-08-class-git.git
cd batch-08-class-git
git status
git diff README.md
git add README.md        # or: git add -A
git diff --staged
git commit -m "Improve README: add clone and usage examples"
git push -u origin feature/edit-readme
```

## Create a repo locally and push to GitHub (from scratch)
```bash
mkdir my-new-repo && cd my-new-repo
git init
git branch -M main
echo "Hello GitHub" > README.md
git add .
git commit -m "Initial commit"
gh repo create my-new-repo --public --source=. --remote=origin   # or --private
git push -u origin main
git remote -v
```

## Merge one branch into another
```bash
git checkout feature/my-change-01
git fetch origin
git pull --rebase origin feature/my-change-01
git merge feature/my-change-02
```

## Revert and reset
```bash
# Safe revert (new commit)
git revert -m 1 7f076de5fed8cf5ac9cb544fa0f6e28e4243ed6d
git push origin main

# Hard reset (rewrites history; caution)
git reset --hard 38f5d6
git push origin main --force
```

## Restore file from main
```bash
git checkout origin/main -- file.txt
git add file.txt
git commit
```

---

# Forking Workflow (summary)
1) Fork the repository on GitHub (creates your 'origin' fork).  
2) Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/REPO_NAME.git
   cd REPO_NAME
   ```
3) Add upstream remote (original repo):
   ```bash
   git remote add upstream https://github.com/ORIGINAL_OWNER/REPO_NAME.git
   git remote -v
   ```
4) Stay updated:
   ```bash
   git fetch upstream
   git checkout main
   git merge upstream/main    # or: git pull upstream main
   git push origin main
   ```
5) Create a feature branch:
   ```bash
   git checkout -b feature/my-new-change
   ```
6) Commit work, push to your fork, open PR against upstream 'main'.
7) After review, push fixes; PR updates automatically.
8) Maintainer merges; repeat step 4 to resync your fork.

Why this is safe: you cannot force-push to upstream; all changes flow through PRs.

---

# Deploying a Node.js App to Heroku (from an empty repo)

## Prerequisites
- Git
- Node.js & npm
- Heroku CLI

## Steps
```bash
# Create project
mkdir my-heroku-app && cd my-heroku-app
git init
npm init -y
npm install express
```

Create `package.json` (example):
```json
{
  "name": "my-heroku-app",
  "version": "1.0.0",
  "description": "Simple Node.js app for Heroku",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

Create 'server.js':
```js
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Hello World! Your Node.js app is running on Heroku.');
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

Create 'Procfile':
```
web: node server.js
```

Create '.gitignore':
```
node_modules
.env
```

Commit and deploy:
```bash
git add .
git commit -m "Initial commit: Node.js Express app ready for Heroku"
heroku login
heroku create
git push heroku main   # use master:main if your branch is master
heroku logs --tail     # optional
```

Expected structure:
```
my-heroku-app/
- Procfile
- package.json
- server.js
- node_modules/ (ignored)
- .gitignore
```

---

## Author
Md. Sarowar Alam  
Lead DevOps Engineer, Hogarth Worldwide  
Email: sarowar@hotmail.com  
LinkedIn: https://www.linkedin.com/in/sarowar/

---
