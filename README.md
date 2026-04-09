# Portfolio Site

Personal site and blog built with Jekyll. Deployed via GitHub Pages.

## Local development

### Prerequisites
- Ruby 3.x
- Bundler

### Setup

```bash
gem install bundler
bundle install
bundle exec jekyll serve
```

Site runs at `http://localhost:4000`.

## Structure

```
.
├── _config.yml          # Site configuration
├── _layouts/            # Page templates
│   ├── default.html     # Base layout (nav + footer)
│   ├── post.html        # Blog post layout
│   └── project.html     # Project page layout
├── _posts/              # Blog posts (YYYY-MM-DD-title.md)
├── _projects/           # Project pages
├── assets/
│   └── css/
│       └── main.scss    # All styles
├── index.html           # Homepage
├── blog.html            # Post index
├── projects.html        # Projects index
└── about.md             # About page
```

## Writing a post

Create a file in `_posts/` named `YYYY-MM-DD-your-title.md`:

```markdown
---
layout: post
title: "Your post title"
date: 2026-04-08
categories: [category1, category2]
description: One sentence summary shown on the homepage.
---

Your content here.
```

## Adding a project

Create a file in `_projects/` named `project-name.md`:

```markdown
---
layout: project
title: "Project Name"
description: Short description shown on the projects grid.
category: ops        # ops, sec, or auto (controls tag color)
tag_label: "PowerShell · Intune"
stack: [PowerShell, Intune]
github: https://github.com/yourusername/repo
---

Longer description and writeup here.
```

## Deploying to GitHub Pages

1. Create a repo named `yourusername.github.io`
2. Push this repo to it
3. Go to Settings → Pages → Source: Deploy from branch → main
4. Site is live at `https://yourusername.github.io`

GitHub Pages builds Jekyll automatically on push — no build step needed.
