# Windows Subsystem for Linux

Quick steps to enable WSL, set up Ubuntu, and common Git/GitHub workflows.

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
wsl --unregister Ubuntu-24.04
```

## Install Git on Ubuntu
```bash
sudo apt install git -y
git --version
```

## Git Workflow Snippets
```bash
# Clone a specific branch
git clone --branch feature/test-change-02 --single-branch git@github.com:md-sarowar-alam/batch-08-class-git.git
# Clone default/main
git clone git@github.com:md-sarowar-alam/batch-08-class-git.git

# Create a new branch (avoid working directly on main)
git checkout -b feature/edit-readme

# Edit and inspect changes
vi README.md
git status
git diff README.md

# Stage changes
git add README.md         # or: git add -A
git diff --staged         # confirm what will be committed

# Commit
git commit -m "Improve README: add clone and usage examples"

# Push branch
git push -u origin feature/edit-readme

# Merge main into your branch (simple & safe)
git checkout feature/test-change-02
git pull origin main      # or: git merge origin/main

# Rebase branch onto main (keep history linear)
git fetch origin
git rebase origin/main
# Commit or stash before rebase if needed
git add .
git commit -m "WIP"
git rebase origin/main

# Reset branch to main (destructive; force-push required)
git checkout feature/my-change
git fetch origin
git reset --hard origin/main
git push --force origin feature/my-change

# Merge one branch into another
git checkout feature/my-change-01
git fetch origin
git pull --rebase origin feature/my-change-01
git merge feature/my-change-02

# Revert a commit safely (creates a new commit)
git revert -m 1 7f076de5fed8cf5ac9cb544fa0f6e28e4243ed6d
git push origin main

# Cleanup branches
git branch -d feature/my-change-01             # delete local
git branch -D feature/my-change-01             # force delete local
git push origin --delete feature/my-change-01  # delete remote

# Restore a file from main
git checkout origin/main -- file.txt
git add file.txt
git commit
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
```

## Create a Repo Locally and Push to GitHub
```bash
# Create project folder
mkdir my-new-repo
cd my-new-repo

# Initialize Git
git init
git branch -M main

# Add a file and commit
echo "Hello GitHub" > README.md
git add .
git commit -m "Initial commit"

# Create remote repo via GitHub CLI
gh repo create my-new-repo --public --source=. --remote=origin
# or private
gh repo create my-new-repo --private --source=. --remote=origin

# Push to GitHub
git push -u origin main
git remote -v   # verify remotes
```

---

## Author
Md. Sarowar Alam  
Lead DevOps Engineer, Hogarth Worldwide  
Email: sarowar@hotmail.com  
LinkedIn: https://www.linkedin.com/in/sarowar/

---
