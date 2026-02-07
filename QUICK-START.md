# Quick Start Checklist

## ✅ Setup Steps (5 minutes)

### Step 1: Create GitHub Repository
- [ ] Go to github.com and sign in
- [ ] Click "New repository"
- [ ] Name it `username.github.io` (replace username with YOUR GitHub username)
  - OR name it anything else like `teaching-resources` for a project site
- [ ] Make it Public
- [ ] Click "Create repository"

### Step 2: Upload Files
- [ ] Download all files from this package
- [ ] On your new GitHub repository page, click "uploading an existing file"
- [ ] Drag and drop ALL files (keeping the folder structure):
  - `_config.yml`
  - `index.html`
  - `README.md`
  - The entire `_resources` folder
- [ ] Click "Commit changes"

### Step 3: Enable GitHub Pages
- [ ] Go to Settings (top of repository page)
- [ ] Click "Pages" in the left sidebar
- [ ] Under "Source", select "Deploy from a branch"
- [ ] Select "main" branch and "/ (root)"
- [ ] Click "Save"

### Step 4: Configure (If NOT using username.github.io)
If your repository is NOT named `username.github.io`:
- [ ] Edit `_config.yml`
- [ ] Change `baseurl: ""` to `baseurl: "/your-repo-name"`
- [ ] Commit the change

### Step 5: Wait and Visit
- [ ] Wait 1-2 minutes for GitHub to build your site
- [ ] Visit your site:
  - User site: `https://username.github.io/`
  - Project site: `https://username.github.io/repo-name/`

## ✅ Adding Your First Resource

### Understanding Folder Organization

Resources are **automatically organized by folder structure**:

- `_resources/English/Barbie/` → Creates "English / Barbie" section
- `_resources/History/WWII/` → Creates "History / WWII" section
- `_resources/standalone.html` → Appears in "General Resources"

### Method 1: Web Interface (Easiest)

**For a new category:**
1. [ ] Go to your repository on GitHub
2. [ ] Click "Add file" → "Create new file"
3. [ ] Type the path: `_resources/English/Barbie/resource-name.html`
   - The `/` creates folders automatically!
4. [ ] Paste your complete HTML (with front matter)
5. [ ] Click "Commit changes"
6. [ ] Wait 1 minute, refresh your site - new section appears!

**To add to existing category:**
1. [ ] Navigate to the folder (e.g., click `_resources` → `English` → `Barbie`)
2. [ ] Click "Add file" → "Upload files"
3. [ ] Upload your `.html` file
4. [ ] Commit and wait 1 minute

### Method 2: GitHub Desktop
1. [ ] Install GitHub Desktop
2. [ ] Clone your repository
3. [ ] Create folders: `_resources/Subject/Topic/`
4. [ ] Add `.html` file to that folder
5. [ ] Commit and push

## ✅ Your Resource File Template

Every file in `_resources/` needs this at the top:

```html
---
title: "Your Resource Title"
subject: "English"
year_level: "9"
topic: "Your Topic"
description: "Brief description"
date: 2025-02-07
---
<!DOCTYPE html>
<html lang="en">
<!-- Your full HTML goes here -->
</html>
```

## 🎉 That's It!

Once set up, just:
1. Create new HTML resource files with front matter
2. Upload to `_resources/` folder
3. They automatically appear on your homepage!

No more manual directory editing! 🚀
