# MkDocs Documentation Workflow (pve1 + GitHub Pages)
This guide explains the full workflow for editing, previewing, and publishing MkDocs-based documentation site from pve1 (homelab server) using GitHub for version control and hosting.

---

## File Location
This file belongs in the repo:

```markdown
homelab-docs/
└── docs/
    └── guides/
        └── mkdocs_workflow_guide.md
```

---

## Prerequisites
MkDocs and Material installed in a Python virtual environment (~/venvs/mkdocs)
- Your GitHub repo cloned at: ~/Projects/homelab-docs
- You can SSH into pve1 or access it via VS Code Remote SSH
- GitHub Pages is enabled on your repo (gh-pages branch)

---

## Recommended Workflow

### 1. SSH into pve1
From Mac/PC terminal:

```bash
ssh user@192.168.1.2
```
Or open VS Code → Remote SSH → user@192.168.1.2 
    
### 2. Activate the Virtual Environment
In the pve1 terminal:

```bash
source ~/venvs/mkdocs/bin/activate
```
Your prompt should change to `(mkdocs) user@pve1:~$`

### 3. Navigate to Your Repo
```bash
cd ~/Projects/homelab-docs
```

### 4. Edit Markdown Docs
Use VS Code or terminal editors (like nano, vim) to update .md files inside:

```markdown
docs/
├── index.md
├── infrastructure/
├── services/
├── monitoring/
└── guides/
```
### 5. Preview Site Locally (Optional)
Use MkDocs’ live server:

```bash
mkdocs serve
```

View the site at:
http://192.168.1.2:8000

> ❗ Your terminal will be locked while serving. Use a new tab to keep editing, or Ctrl+C to stop it.

### 6. Save and Commit Changes to Git
After editing content:

```bash
git add .
git commit -m "Update docs: brief summary of what changed"
git push origin main
# This updates your GitHub repo but does not update the live site yet.
```

### 7. Deploy to GitHub Pages
Run:

```bash
mkdocs gh-deploy
```

This will:
- Build the static site
- Push it to the gh-pages branch
- Update your public site at `https://username.github.io/homelab-docs/`

---

## Summary
- **Enter virtual env:** `source ~/venvs/mkdocs/bin/activate`
- **Navigate to repo:** `cd ~/Projects/homelab-docs`
- **Preview site:** `mkdocs serve`
- **Commit changes:** `git add . && git commit -m "..." && git push origin main`
- **Deploy site:** `mkdocs gh-deploy`
- **View live** `https://username.github.io/homelab-docs/`
