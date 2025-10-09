# GitHub Pages Setup Instructions

Quick guide to deploy the documentation site.

---

## 🚀 One-Time Setup

### 1. Push to GitHub

```bash
cd /home/pcone/code/github.com/tfgrid-compose/tfgrid-docs

# Add all files
git add .

# Commit
git commit -m "Add comprehensive documentation with MkDocs Material

- Landing page with project overview
- Getting started guides (introduction, installation, quickstart)
- Current status documentation (v1.0.0)
- Planned features roadmap (gateway Q4 2025, k3s Q1 2026)
- Source repository acknowledgment (mik-tf, ucli-tools)
- MkDocs Material configuration
- GitHub Actions auto-deployment
- Navigation structure for all planned docs"

# Push to GitHub
git push origin main
```

### 2. Enable GitHub Pages

1. Go to: `https://github.com/tfgrid-compose/tfgrid-docs/settings/pages`
2. Under "Source", select: **Deploy from a branch**
3. Select branch: **gh-pages**
4. Select folder: **/ (root)**
5. Click **Save**

### 3. Wait for Deployment

- GitHub Actions will automatically build and deploy
- Check progress: `https://github.com/tfgrid-compose/tfgrid-docs/actions`
- First deployment takes ~2-3 minutes

### 4. Access Documentation

Your docs will be live at:
- **Default:** `https://tfgrid-compose.github.io/tfgrid-docs/`
- **Custom domain (optional):** `https://docs.tfgrid-compose.io/`

---

## 🔧 Local Development

### Install MkDocs Material

```bash
# Using pip
pip install mkdocs-material
pip install mkdocs-minify-plugin

# Or using pipenv
pipenv install mkdocs-material mkdocs-minify-plugin
```

### Serve Locally

```bash
cd /home/pcone/code/github.com/tfgrid-compose/tfgrid-docs

# Start development server
mkdocs serve

# Open in browser: http://127.0.0.1:8000
```

**Features:**
- ✅ Live reload on file changes
- ✅ Instant search preview
- ✅ Full theme preview
- ✅ Mobile responsive testing

### Build Static Site

```bash
# Build for production
mkdocs build

# Output in: ./site/

# Test the build
cd site
python -m http.server 8000
```

---

## 📝 Making Updates

### Workflow

```bash
# 1. Edit markdown files
nano getting-started/quickstart.md

# 2. Preview locally
mkdocs serve

# 3. Commit and push
git add .
git commit -m "Update quickstart guide"
git push origin main

# 4. GitHub Actions automatically deploys
```

### Auto-Deployment

Every push to `main` triggers:
1. ✅ Checkout repository
2. ✅ Install Python & MkDocs
3. ✅ Build documentation
4. ✅ Deploy to gh-pages branch
5. ✅ Live in ~2 minutes

---

## 🎨 Customization

### Change Theme Colors

Edit `mkdocs.yml`:

```yaml
theme:
  palette:
    primary: indigo  # Change to: blue, red, green, etc.
    accent: indigo   # Accent color
```

### Add Custom Domain

1. Add `CNAME` file:
   ```bash
   echo "docs.tfgrid-compose.io" > CNAME
   git add CNAME
   git commit -m "Add custom domain"
   git push
   ```

2. Configure DNS:
   - Add CNAME record: `docs` → `tfgrid-compose.github.io`

3. Enable in GitHub Pages settings

### Add Google Analytics

Edit `mkdocs.yml`:

```yaml
extra:
  analytics:
    provider: google
    property: G-XXXXXXXXXX
```

---

## 🐛 Troubleshooting

### Build Fails

```bash
# Check for syntax errors
mkdocs build --strict

# Common issues:
# - Missing files in nav
# - YAML syntax errors
# - Broken internal links
```

### Pages Not Updating

```bash
# Clear cache and rebuild
rm -rf site/
mkdocs build --clean

# Force push
git push origin main --force
```

### Local Server Won't Start

```bash
# Check if port is in use
lsof -i :8000

# Use different port
mkdocs serve -a 127.0.0.1:8001
```

---

## 📚 Adding New Pages

### 1. Create Markdown File

```bash
# Create new guide
nano guides/my-new-guide.md
```

### 2. Add to Navigation

Edit `mkdocs.yml`:

```yaml
nav:
  - Guides:
    - My New Guide: guides/my-new-guide.md
```

### 3. Preview and Deploy

```bash
mkdocs serve  # Preview
git add .
git commit -m "Add new guide"
git push
```

---

## ✅ Verification Checklist

After deployment, verify:

- [ ] Site loads at GitHub Pages URL
- [ ] Navigation works (all sections clickable)
- [ ] Search works (try searching "deploy")
- [ ] Dark mode toggle works
- [ ] Mobile view is responsive
- [ ] Code blocks have copy buttons
- [ ] Internal links work
- [ ] Edit on GitHub links work

---

## 🔗 Useful Links

- **Live Site:** https://tfgrid-compose.github.io/tfgrid-docs/
- **GitHub Repo:** https://github.com/tfgrid-compose/tfgrid-docs
- **Actions:** https://github.com/tfgrid-compose/tfgrid-docs/actions
- **MkDocs Material:** https://squidfunk.github.io/mkdocs-material/
- **MkDocs:** https://www.mkdocs.org/

---

**Ready to deploy!** Push to GitHub and enable Pages. 🚀
