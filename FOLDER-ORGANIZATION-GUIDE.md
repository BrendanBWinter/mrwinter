# Folder Organization Visual Guide

This guide shows how your folder structure translates to the website organization.

## 📁 How Folders Become Sections

### Your Folder Structure
```
_resources/
├── English/
│   ├── Barbie/
│   │   ├── film-analysis.html
│   │   └── character-analysis.html
│   └── Persuasion/
│       └── persuasive-techniques.html
├── History/
│   └── WWII/
│       └── causes-of-wwii.html
└── standalone-resource.html
```

### How It Appears on Your Website

```
┌─────────────────────────────────────────────────┐
│ ▼  English / Barbie              2 resources    │  ← Expandable section
├─────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────┐   │
│  │ 📄 Barbie Film Analysis                  │   │
│  │ Year 10 | Film Study                     │   │
│  │ Analyze themes and symbolism...          │   │
│  │ [View Resource →]                        │   │
│  └──────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────┐   │
│  │ 📄 Barbie Character Analysis             │   │
│  │ Year 10 | Film Study                     │   │
│  │ Deep dive into character development...  │   │
│  │ [View Resource →]                        │   │
│  └──────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────┐
│ ▶  English / Persuasion          1 resource     │  ← Collapsed section
└─────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────┐
│ ▶  History / WWII                1 resource     │  ← Collapsed section
└─────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────┐
│ ▼  General Resources             1 resource     │  ← Root-level files
├─────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────┐   │
│  │ 📄 Standalone Resource                   │   │
│  │ [metadata from front matter]             │   │
│  └──────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

## 🎯 Organizational Strategies

### Strategy 1: Subject → Topic
**Best for:** General curriculum organization
```
_resources/
├── English/
│   ├── Shakespeare/
│   ├── Poetry/
│   ├── Creative-Writing/
│   └── Film-Analysis/
├── History/
│   ├── Ancient-Rome/
│   ├── Medieval-Europe/
│   └── WWII/
└── Science/
    ├── Biology/
    ├── Chemistry/
    └── Physics/
```

**Result on website:**
- "English / Shakespeare"
- "English / Poetry"
- "History / Ancient-Rome"
- etc.

### Strategy 2: Year Level → Subject
**Best for:** Schools with year-specific curriculum
```
_resources/
├── Year-9/
│   ├── English/
│   ├── History/
│   └── Science/
├── Year-10/
│   ├── English/
│   ├── History/
│   └── Science/
└── Year-11/
    ├── English/
    └── History/
```

**Result on website:**
- "Year-9 / English"
- "Year-9 / History"
- "Year-10 / English"
- etc.

### Strategy 3: Unit-Based
**Best for:** Thematic or unit-based teaching
```
_resources/
├── Identity-and-Belonging/
├── Social-Justice/
├── Environmental-Issues/
└── Technology-and-Society/
```

**Result on website:**
- "Identity-and-Belonging"
- "Social-Justice"
- etc.

### Strategy 4: Hybrid Approach
**Best for:** Flexibility
```
_resources/
├── English/
│   ├── Year-9/
│   │   ├── Persuasion/
│   │   └── Creative-Writing/
│   └── Year-10/
│       ├── Shakespeare/
│       └── Film-Analysis/
├── History/
│   └── Modern-History/
└── cross-curricular.html
```

**Result on website:**
- "English / Year-9 / Persuasion"
- "English / Year-10 / Shakespeare"
- "History / Modern-History"
- "General Resources" (for cross-curricular.html)

## 💡 Best Practices

### Naming Folders

✅ **Good folder names:**
- `Shakespeare` - Clear and concise
- `WWII` - Standard abbreviations
- `Creative-Writing` - Hyphens for multi-word
- `Romeo-and-Juliet` - Specific topic

❌ **Avoid:**
- `shakespeare stuff` - Contains space
- `World War 2` - Spaces cause issues
- `Module 1` - Not descriptive

### Organizing Multiple Resources

If you have many resources on one topic, consider:

```
_resources/
└── English/
    └── Barbie/
        ├── 01-film-analysis.html
        ├── 02-character-analysis.html
        ├── 03-themes.html
        ├── 04-symbolism.html
        └── 05-essay-prompts.html
```

**Tips:**
- Number files (01, 02, 03) if order matters
- Use descriptive filenames
- They'll appear sorted by date (from front matter)

## 🔄 Reorganizing Later

**Want to move resources?** Just reorganize the folders!

**Before:**
```
_resources/
└── shakespeare-resource.html
```

**After:**
```
_resources/
└── English/
    └── Shakespeare/
        └── shakespeare-resource.html
```

1. Move the file to the new folder structure
2. Commit and push
3. The website automatically updates to show the new organization!

## 🎨 Visual Customization

The category headers use your school's branding:
- **Collapsed:** Navy blue left border
- **Hovered:** Green left border
- **Expanded:** Gold left border
- **Font:** Roboto Slab headings

Students see:
- Resource count badge
- Clickable headers
- Smooth expand/collapse animation
- Their selection is remembered for next visit

## 📋 Quick Reference

| Folder Structure | Website Section |
|-----------------|-----------------|
| `_resources/English/Barbie/` | English / Barbie |
| `_resources/History/WWII/Ancient/` | History / WWII / Ancient |
| `_resources/Year-10/Science/` | Year-10 / Science |
| `_resources/general.html` | General Resources |

**Key Rule:** Every folder in the path becomes part of the section name, separated by " / "
