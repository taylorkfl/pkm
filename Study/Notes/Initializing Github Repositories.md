---
type: Notes
status: current
last_reviewed: 2026-07-01
tags:
  - study
  - Github
---
his repo is on branch main, has no GitHub remote configured, and the files are still untracked.
interview-demo:
```
# Confirm what is tracked/untracked (confirm .env is ignored)
git status

# Stage changes
git add .

# Commit changes
git commit -m "Initial commit"

# On github.com setup an empty repo, then connect it manually:

git remote add origin git@github.com:YOUR_USERNAME/kfl-interview-demo.git
git push -u origin main
```