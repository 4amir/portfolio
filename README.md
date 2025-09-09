# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a Hugo static site generator portfolio website using the PaperMod theme. The site is configured for a personal portfolio with blog posts, projects showcase, and professional profile information.

## Development Commands

### Core Hugo Commands
```bash
# Start development server with live reload (most common command)
hugo server -D

# Start development server with drafts and bind to all interfaces (for testing on other devices)
hugo server -D --bind=0.0.0.0 --port=1313

# Build the site for production (outputs to public/)
hugo

# Build with drafts included
hugo -D

# Clean generated files and module cache
hugo mod clean

# Check for broken links
hugo --gc --minify --enableGitInfo -b https://yourusername.github.io/portfolio/
```

### Content Management
```bash
# Create a new blog post
hugo new posts/my-new-post.md

# Create a new project page
hugo new projects/my-project.md

# Create a new page
hugo new about.md

# List all content
find content -type f | sort
```

### Theme and Submodule Management
```bash
# Update the PaperMod theme submodule
git submodule update --remote themes/PaperMod

# Initialize submodules when cloning
git submodule update --init --recursive

# Check theme status
git submodule status
```

### Deployment Commands
```bash
# Deploy to GitHub Pages (if using GitHub Actions)
git add .
git commit -m "Update site content"
git push origin main

# Manual deployment (build and deploy)
hugo --minify
# Then copy the public/ directory to your hosting service
```

## Site Architecture

### Configuration Structure
- **hugo.toml**: Main configuration file with site settings, theme parameters, menu configuration, and social links
- **Profile Mode**: Enabled by default showing author info, subtitle, profile image, and navigation buttons
- **Theme**: Uses PaperMod theme (maintained as a Git submodule)

### Content Organization
```
content/
├── posts/          # Blog posts (appears in /posts/)
├── projects/       # Project showcases (appears in /projects/)
└── about.md        # About page (appears in /about/)
```

### Key Directories
- `archetypes/`: Content templates for new pages (front matter defaults)
- `assets/`: Site assets like images, CSS, JS files
- `layouts/`: Custom layout overrides (currently empty, uses theme defaults)
- `static/`: Static files served at site root (images, etc.)
- `themes/PaperMod/`: Git submodule containing the theme
- `public/`: Generated site (gitignored)
- `resources/`: Cache directory (gitignored)

### Menu Configuration
The site uses Hugo's menu system defined in hugo.toml:
- About (/about/) - weight 10
- Projects (/projects/) - weight 20  
- Blog (/posts/) - weight 30
- Search (/search/) - weight 40

## Content Creation Patterns

### Front Matter Structure
All content uses TOML front matter with these key fields:
```toml
+++
date = "2023-09-08T12:00:00+00:00"
draft = true
title = "My Post Title"
tags = ["tag1", "tag2"]
categories = ["category"]
+++
```

Required fields:
- `date`: Publication date (auto-generated)
- `draft`: Boolean for draft status
- `title`: Page/post title (auto-generated from filename)

Optional fields:
- `tags`: Array of tags for categorization
- `categories`: Array of categories
- `description`: Short summary for SEO
- `showToc`: Boolean to display table of contents

### Profile Mode Configuration
The homepage uses profile mode with:
- Author name and subtitle
- Profile image (expects `images/profile.jpg` in static/)
- Navigation buttons linking to main sections
- Social media icons (GitHub, LinkedIn, Twitter)

## Hugo Version Compatibility

This site is developed with Hugo v0.149.1+extended. The extended version is required for SCSS/SASS processing. You can check your Hugo version with:

```bash
hugo version
```

If you need to install Hugo:
```bash
# macOS
brew install hugo

# Linux
snap install hugo

# Windows
choco install hugo-extended
```

## Deployment Considerations

### Build Output
- Generated site goes to `public/` directory (gitignored)
- Resources cache in `resources/_gen/` (gitignored)
- Site can be deployed to any static hosting service

### Environment Configuration
The current configuration expects:
- Base URL: `https://yourusername.github.io/portfolio/`
- Profile image at `static/images/profile.jpg`
- Social links need to be updated with actual URLs

## Development Workflow

1. Use `hugo server -D` for local development with drafts
2. Create content with `hugo new` command for proper front matter
3. Customize styling by overriding theme files in local `layouts/` or `assets/`
4. Update theme periodically with `git submodule update --remote themes/PaperMod`
5. Build with `hugo` for production deployment

## Theme Customization

### PaperMod Theme Features Enabled
- Reading time display
- Post navigation links
- Breadcrumbs
- Code copy buttons
- Word count
- RSS buttons
- Hugo table of contents

### Override Locations
To customize the theme without modifying the submodule:
- Custom layouts: `layouts/` directory
- Custom assets: `assets/` directory  
- Custom static files: `static/` directory

The theme is maintained as a Git submodule, so direct modifications to `themes/PaperMod/` should be avoided. Instead, override files in the root project directories.

### Common Theme Customizations
```bash
# Override the header
mkdir -p layouts/partials
cp themes/PaperMod/layouts/partials/header.html layouts/partials/

# Override CSS
mkdir -p assets/css
touch assets/css/extended.css  # Add custom CSS here

# Override a specific page template
mkdir -p layouts/_default
cp themes/PaperMod/layouts/_default/single.html layouts/_default/
```

## Troubleshooting

### Common Issues
1. **Theme not loading**: Check that submodules are initialized
   ```bash
   git submodule update --init --recursive
   ```

2. **Content not appearing**: Check for draft status
   ```bash
   grep -r "draft = true" content/
   ```

3. **CSS/SCSS not processing**: Ensure you're using Hugo Extended
   ```bash
   hugo version | grep -i extended
   ```

4. **Site not building**: Check for content format issues
   ```bash
   hugo --logLevel debug
   ```
