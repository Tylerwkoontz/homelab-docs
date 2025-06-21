# Deploying MkDocs with Material Theme

This guide explains how to deploy a professional, searchable documentation site using **MkDocs with the Material theme**, hosted on GitHub Pages. It is intended for use directly on `pve1` or your local development system.

---

## Requirements

- Python 3
- pip
- A GitHub repo: [https://github.com/Tylerwkoontz/homelab-docs](https://github.com/Tylerwkoontz/homelab-docs)

---

## Step 1: Install MkDocs and Material Theme
Due to recent Debian/Ubuntu changes, system-wide pip installs are blocked to protect the OS. We’ll use a Python virtual environment to install MkDocs cleanly.

### 1. Prepare Python Environment
Install the required packages:
```bash
sudo apt update
sudo apt install python3-full python3-venv python3.11-venv -y
```

> `python3-venv` and `python3.11-venv` provide the tools needed for creating Python virtual environments.
> `python3-full` ensures you have all standard Python modules, including `ensurepip`, required to bootstrap `pip` inside virtual environments

### 2. Create and Activate Virtual Environment
```bash
# (Optional) Create a folder to organize all your venvs
mkdir -p ~/venvs

# Create a virtual environment specifically for MkDocs
python3 -m venv ~/venvs/mkdocs

# Activate the virtual environment
source ~/venvs/mkdocs/bin/activate
```
You’ll know it's active when your prompt looks like this:
```bash
(mkdocs) user@pve1:~$
```

### 3. Install MkDocs and Material Theme
Inside the virtual environment:

```bash
pip install mkdocs mkdocs-material
```
Then verify:
```bash
mkdocs --version
```

> ### Reminder:
> Each time you want to edit or serve your docs:
> 
> ```bash
> source ~/venvs/mkdocs/bin/activate
> ```
> To exit:
> 
> ```bash
> deactivate
> ```

---

## Step 2: Clone Your GitHub Repo on pve1

On `pve1`, download your project repo:

```bash
mkdir -p ~/Projects && cd ~/Projects
git clone https://github.com/<username>/homelab-docs.git
cd homelab-docs
```

---

## Step 3: Initialize or Organize the MkDocs Project
> Choose one of the following based on your situation.

### A) If You Already Have a /docs/ Directory
No need to run mkdocs new .. Just ensure your folder layout follows MkDocs structure:

```bash
# Create the required folder structure inside the /docs directory
mkdir -p docs/infrastructure docs/services/pihole docs/monitoring docs/guides

# Move Proxmox or infrastructure-related markdown files into the correct folder
mv infrastructure/*.md docs/infrastructure/ 2>/dev/null

# Move Pi-hole-related markdown files into the services/pihole directory
mv services/pihole/*.md docs/services/pihole/ 2>/dev/null

# Move Grafana or other monitoring-related markdown files
mv monitoring/*.md docs/monitoring/ 2>/dev/null

# Move general guide markdown files into the guides folder
mv guides/*.md docs/guides/ 2>/dev/null

# Use the main README as the homepage of your MkDocs site
mv README.md docs/index.md
```

### B) If You're Starting from Scratch
Use `mkdocs new .` to initialize a new project structure:

```bash
mkdocs new .
```
This creates:

```
mkdocs.yml        # Site configuration
docs/
└── index.md      # Default homepage
```

You can then add your custom folders like:

```bash
mkdir -p docs/infrastructure docs/services/pihole docs/monitoring docs/guides
```
And move your markdown files accordingly.

---

## Step 4: Configure the Material Theme

Edit `mkdocs.yml`:

```yml
site_name: Homelab Docs
site_url: http://192.168.1.2:8000
theme:
  name: material

nav:
  - Home: index.md
  - Infrastructure:
      - Proxmox Setup: infrastructure/proxmox_setup.md
  - Services:
      - Pi-hole: services/pihole/config_notes.md
  - Monitoring:
      - Grafana Setup: monitoring/grafana_setup.md
```

---

## Step 5: Preview the Site Locally

Start the live server:

```bash
mkdocs serve
```

Then open in your browser:
```
http://192.168.1.2:8000
```

---

## Step 6: Deploy to GitHub Pages
### 6.1 Install the GitHub deploy plugin
In your virtual environment:
```bash
pip3 install mkdocs-material[gh-deploy]
```

### 6.2 Deploy the site
Build and push your documentation to the gh-pages branch of your GitHub repo:
```bash
mkdocs gh-deploy
```

When prompted for GitHub credentials:

**Username**: your GitHub username (e.g., tylerwkoontz)

**Password**: use a Personal Access Token (PAT) instead of your actual password.

> ❗ GitHub no longer supports password authentication for Git over HTTPS. You must use a Personal Access Token (PAT) with at least the repo or public_repo scope enabled.

### 6.3 (Optional) Cache your GitHub credentials
To avoid re-entering your PAT every time:

```bash
git config --global credential.helper store
# This saves your credentials in plaintext at ~/.git-credentials. Only use this on secure, personal systems.
```


---

## Step 7: Enable GitHub Pages

1. Go to your repo on GitHub: `https://github.com/<username>/homelab-docs`
2. Navigate to: **Settings → Pages**
3. Under **Source**, choose:
   - Branch: `gh-pages`
   - Folder: `/ (root)`
4. Click **Save**

Your site will be live at:
```
https://<username>.github.io/homelab-docs/
```

---

## Step 8: Workflow for Updating Docs
Here's how to maintain and update your site:

### 1. Edit Locally with Live Preview
Use MkDocs' live server to see changes in real-time:
```bash
mkdocs serve
```
View locally at http://192.168.1.2:8000

### 2. Save and Push Changes to GitHub
After editing markdown files or the config:
```bash
git add .
git commit -m "Update docs or config"
git push
# This saves your edits to GitHub but does not publish them yet.
```

### 3. Deploy to GitHub Pages
When you're ready to update your live site:
```bash
mkdocs gh-deploy
# This builds your static site and pushes it to the gh-pages branch.
```

---

# Optional Enhancements
Take your MkDocs site to the next level with these enhancements:

---

## Full-Text Search (Built-In)
No setup required — Material for MkDocs includes a client-side search engine by default.

**What it does:**
- Instantly searches page titles and content
- Provides dropdown suggestions as you type
- Works offline — no external dependencies

---

## Custom Logo and Favicon
Customize your site branding:
```yml
theme:
  name: material
  logo: images/logo.png         # Appears in the upper-left corner
  favicon: images/favicon.ico   # Appears in browser tab
```
**What it does:**
- Gives your docs a personalized and professional look
- Reinforces branding, especially if used for public or client-facing projects

**Place the image files in:**
```bash
docs/images/logo.png
docs/images/favicon.ico
```

---

## Structure Long Pages with Headings
Use markdown headers to divide content:
```markdown
# H1 - Top-level title
## H2 - Section
### H3 - Subsection
```
**What it does:**
- Creates an auto-generated sidebar TOC (Table of Contents)
- Improves readability and navigation
- Helps readers scan content quickly

---

## Use Checklists for Task Tracking
Use GitHub-style task lists:

```markdown
- [x] Install MkDocs
- [ ] Configure theme
- [ ] Write documentation
```
**What it does:**
- Makes setup or troubleshooting guides interactive
- Can serve as live checklists for system setup, deployments, etc.

---

## Use Tables for Data Presentation
Organize comparisons or structured info:

```markdown
| Feature       | Enabled | Notes                     |
|---------------|---------|---------------------------|
| Snapshots     | ✅       | ZFS hourly, daily, weekly |
| Monitoring    | ❌       | Grafana setup pending     |
```

**What it does:**
- Makes dense information easier to scan
- Adds clarity to service lists, features, or status reports

---

## Enable Dark Mode Toggle
Material supports built-in light/dark theme switching:
```yaml
theme:
  name: material
  palette:
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: indigo
      accent: indigo
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: indigo
      accent: indigo
```
**What it does:**
- Automatically adapts to system dark/light mode
- Adds a toggle button to the header

---

## Add a Custom 404 Page
Create a helpful error page:
```bash
touch docs/404.md
```
**What it does:**
- Improves user experience when a page is missing
- Prevents confusion or dead ends in navigation

---

## Add Plugins (Advanced)
Extend MkDocs with features like:

`mkdocs-awesome-pages-plugin:` Custom sort nav order

`mkdocs-git-revision-date-localized-plugin:` Show last updated timestamp

`mkdocs-minify-plugin:` Reduce page size for faster loads

**What they do:**
Enhance control, performance, and versioning in large documentation sets.

---

## Summary

| Task | Command |
|------|---------|
| Install MkDocs | `pip3 install mkdocs mkdocs-material` |
| Clone repo | `git clone https://github.com/Tylerwkoontz/homelab-docs.git` |
| Create project | `mkdocs new .` or manually organize |
| Preview locally | `mkdocs serve` |
| Deploy | `mkdocs gh-deploy` |
| Live site | `https://<username>.github.io/homelab-docs/` |
