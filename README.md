# Teaching Resources - GitHub Pages Setup Guide

This repository contains an auto-updating directory of teaching resources for The Geelong College, following the school's brand guidelines.

## 📁 File Structure

```
your-repo/
├── _config.yml              # Jekyll configuration
├── index.html               # Auto-updating homepage with folder organization
├── _resources/              # Your teaching resource files go here
│   ├── English/             # Subject folder
│   │   ├── Barbie/          # Topic subfolder
│   │   │   ├── barbie-film-analysis.html
│   │   │   └── character-analysis.html
│   │   └── Persuasion/      # Another topic subfolder
│   │       └── persuasive-language-techniques.html
│   ├── History/             # Another subject folder
│   │   └── WWII/            # Topic subfolder
│   │       └── causes-of-wwii.html
│   └── standalone-resource.html  # Can also have resources in root
└── README.md                # This file
```

## 📂 How Folder Organization Works

The homepage **automatically organizes resources based on their folder structure**. Resources are grouped into expandable/collapsible sections based on their folder path.

### Example Organization

**Folder:** `_resources/English/Barbie/`
- Creates a section called "English / Barbie"
- All resources in this folder appear together
- Section shows resource count (e.g., "2 resources")
- Click header to expand/collapse section

**Folder:** `_resources/English/Persuasion/`
- Creates a separate section "English / Persuasion"
- Independent from the Barbie section

**Folder:** `_resources/History/WWII/`
- Creates section "History / WWII"
- Supports any subject/topic combination

### Folder Structure Options

You have complete flexibility in how you organize:

1. **By Subject and Topic** (recommended)
   ```
   _resources/
   ├── English/
   │   ├── Shakespeare/
   │   ├── Persuasion/
   │   └── Poetry/
   ├── History/
   │   ├── WWII/
   │   └── Ancient-Rome/
   └── Science/
       └── Biology/
   ```

2. **By Year Level and Subject**
   ```
   _resources/
   ├── Year-9/
   │   ├── English/
   │   └── History/
   └── Year-10/
       ├── English/
       └── Science/
   ```

3. **By Unit or Module**
   ```
   _resources/
   ├── Unit-1-Identity/
   ├── Unit-2-Social-Change/
   └── Unit-3-Global-Issues/
   ```

4. **Mixed Approach**
   ```
   _resources/
   ├── English/Barbie/          # Nested folders
   ├── History/WWII/
   └── general-resources.html   # Root-level resource
   ```

### Important Notes

- **Automatic Grouping**: Resources are automatically grouped by their folder path
- **Expandable Sections**: Each folder group has a clickable header to show/hide resources
- **Resource Count**: Shows how many resources are in each section
- **Remembers State**: The expand/collapse state is saved in your browser
- **First Section Opens**: By default, the first section is expanded when you first visit
- **Root Level**: Resources placed directly in `_resources/` appear in a "General Resources" section

## 🚀 Quick Start

### 1. Initial Setup

1. **Create a new GitHub repository** (or use an existing one)
   - Name it `username.github.io` for a user site, or any name for a project site
   - Make it public

2. **Upload these files to your repository:**
   - `_config.yml`
   - `index.html`
   - Create a folder called `_resources`

3. **Enable GitHub Pages:**
   - Go to repository Settings → Pages
   - Under "Source", select "Deploy from a branch"
   - Select the `main` branch and `/ (root)` folder
   - Click Save

4. **Wait 1-2 minutes** for GitHub Pages to build your site
   - Your site will be available at `https://username.github.io/` or `https://username.github.io/repo-name/`

### 2. If Using a Project Site (Not username.github.io)

If your repository is NOT named `username.github.io`, you need to update `_config.yml`:

```yaml
baseurl: "/your-repo-name"  # Add your repository name here
```

For example, if your repo is called `teaching-resources`:
```yaml
baseurl: "/teaching-resources"
```

## 📝 Adding New Resources

### Step 1: Choose Your Folder

Decide where the resource belongs in your folder structure:

- **Existing category?** Add to that folder (e.g., `English/Barbie/`)
- **New category?** Create a new folder (e.g., `English/Romeo-and-Juliet/`)
- **General resource?** Place directly in `_resources/`

### Step 2: Add the Resource File

### Method 1: Upload Directly to GitHub

1. Go to your repository on GitHub
2. Navigate to the folder: Click `_resources` → click your subject folder → click your topic folder
   - If the folder doesn't exist, create it using "Create new file" and add `/` to create folders
3. Click "Add file" → "Upload files"
4. Upload your `.html` resource file(s)
5. Commit the changes
6. Wait 1-2 minutes for the site to rebuild
7. Your new resource will automatically appear in the correct category!

**Creating a new folder through GitHub:**
1. Click "Add file" → "Create new file"
2. Type: `English/NewTopic/resource-name.html`
   - The `/` characters create folders automatically
3. Paste your file content
4. Commit the file

### Method 2: Git Command Line

```bash
# Clone your repository
git clone https://github.com/username/repo-name.git
cd repo-name

# Create a new topic folder (if needed)
mkdir -p _resources/English/Barbie

# Add your resource file
cp your-new-resource.html _resources/English/Barbie/

# Commit and push
git add _resources/English/Barbie/your-new-resource.html
git commit -m "Add Barbie resource"
git push origin main
```

### Method 3: GitHub Desktop

1. Clone repository to your computer
2. Navigate to `_resources` folder
3. Create subfolders as needed (e.g., `English/Barbie`)
4. Copy your HTML file into the appropriate folder
5. Commit and push through GitHub Desktop

## 📄 Resource File Format

Every resource file in `_resources/` must have "front matter" at the top (between the `---` lines). This metadata is what makes the auto-directory work.

### Required Front Matter

```html
---
title: "Your Resource Title"
subject: "English"
year_level: "9"
topic: "Topic Name"
description: "Brief description of what this resource covers"
date: 2025-02-07
---
<!DOCTYPE html>
<html lang="en">
<!-- Your full HTML resource goes here -->
</html>
```

### Front Matter Fields

- **title**: The name of your resource (shows as heading on directory)
- **subject**: Subject area (shows as colored tag)
- **year_level**: Year level (shows as colored tag)
- **topic**: Specific topic (shows as gray tag)
- **description**: Brief description (shows in card)
- **date**: Date in YYYY-MM-DD format (used for sorting - newest first)

### Optional Front Matter

You can add any other metadata you want:
```yaml
---
title: "Persuasive Writing"
subject: "English"
year_level: "9"
topic: "Persuasion"
description: "Learn persuasive techniques"
date: 2025-02-07
duration: "60 minutes"
difficulty: "Intermediate"
---
```

## 🎨 Using Your Existing Resource Template

To convert your existing differentiated resource template into a file that works with this system:

1. **Take your complete HTML resource file** (the one with Foundation/Standard/Advanced levels)

2. **Add front matter to the very top:**

```html
---
title: "Persuasive Language Techniques"
subject: "English"
year_level: "9"
topic: "Persuasion"
description: "Identify and analyze persuasive language techniques"
date: 2025-02-07
---
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Your existing head content -->
</head>
<body>
    <!-- Your existing differentiated content -->
</body>
</html>
```

3. **Save it in the `_resources` folder** with a descriptive filename:
   - Good: `persuasive-language-techniques.html`
   - Good: `year9-english-metaphors.html`
   - Avoid spaces: Use hyphens instead

4. **Upload to GitHub** - it will automatically appear on your directory page!

## 🔄 How Auto-Update Works

The homepage uses Jekyll's Liquid templating to:

1. **Find all files** in the `_resources` collection
2. **Sort them** by date (newest first)
3. **Display them** as cards with their metadata
4. **Link to them** automatically

When you add a new file to `_resources/` and push to GitHub:
- GitHub Pages automatically rebuilds the site
- The new resource appears on the homepage
- No manual editing of index.html required!

## 🎯 Tips & Best Practices

### File Naming
- Use lowercase and hyphens: `topic-name.html`
- Be descriptive: `year9-persuasive-techniques.html`
- Avoid spaces and special characters

### Dates
- Use YYYY-MM-DD format: `2025-02-07`
- Resources are sorted newest first
- Update the date when you significantly revise a resource

### Descriptions
- Keep them concise (1-2 sentences)
- Focus on what students will learn
- Avoid HTML in descriptions (plain text only)

### Testing Locally (Optional)
If you want to preview changes before pushing:

```bash
# Install Jekyll (one time)
gem install bundler jekyll

# Run local server
jekyll serve

# View at http://localhost:4000
```

## 🐛 Troubleshooting

### Site not updating?
- Wait 2-3 minutes after pushing changes
- Check GitHub Pages build status in Settings → Pages
- Verify your `_config.yml` has correct `baseurl` if using a project site

### Resource not appearing?
- Check front matter syntax (must have `---` on separate lines)
- Verify file is in `_resources/` folder
- Ensure `title` field exists in front matter
- Check for YAML syntax errors (indentation matters!)

### Styling looks wrong?
- If using a project site, verify `baseurl` in `_config.yml`
- Check that fonts are loading (need internet connection)
- Clear browser cache and hard refresh (Ctrl+Shift+R)

### Links not working?
- For project sites, all internal links need `baseurl`
- Use `{{ resource.url | relative_url }}` in templates
- Use relative paths for images: `./images/photo.jpg`

## 📚 Example Workflow

Here's a complete example of adding a new resource to a specific category:

```bash
# 1. Create your resource file locally
# (Use your existing template and add front matter)

# 2. Navigate to your repo
cd teaching-resources

# 3. Create the folder structure if it doesn't exist
mkdir -p _resources/English/Romeo-and-Juliet

# 4. Copy file to the appropriate folder
cp ~/Desktop/my-new-resource.html _resources/English/Romeo-and-Juliet/themes-analysis.html

# 5. Commit and push
git add _resources/English/Romeo-and-Juliet/themes-analysis.html
git commit -m "Add Romeo and Juliet themes analysis"
git push origin main

# 6. Wait 1-2 minutes, then check your site!
# The new resource will automatically appear under "English / Romeo-and-Juliet"
```

### What Happens Automatically

1. **Folder detected**: System sees `English/Romeo-and-Juliet/`
2. **Category created**: Creates a collapsible section "English / Romeo-and-Juliet"
3. **Resource added**: Your resource appears in that section
4. **Count updated**: Shows "1 resource" in the section header
5. **No manual editing**: You never touch the index.html file!

## 🎓 Next Steps

1. **Create your first resource** using your existing template + front matter
2. **Upload it** to the `_resources` folder
3. **Check your GitHub Pages site** to see it appear automatically
4. **Repeat** for each new resource - no more manual directory updates!

## 📞 Support

If you encounter issues:
- Check the GitHub Pages build log in Settings → Pages
- Verify your YAML front matter syntax
- Ensure files are in the correct folders
- Review the Jekyll documentation: https://jekyllrb.com/docs/

---

**Ready to start?** Just add your first HTML resource to `_resources/` with proper front matter, push to GitHub, and watch it appear on your homepage automatically! 🚀
