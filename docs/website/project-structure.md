# Project Structure

Understanding the organization of the UiASub website codebase.

## Overview

The project follows a clear directory structure that separates content by language, with shared assets and modular components.

```bash
uiasub.github.io/
├── index.html              # Norwegian landing page
├── header.html             # Shared header component
├── footer.html             # Shared footer component
├── service-worker.js       # PWA caching
├── manifest.webmanifest    # PWA manifest
├── robots.txt              # SEO configuration
├── pages/                  # Norwegian content pages
├── en/                     # English version
├── css/                    # Stylesheets
├── js/                     # JavaScript files
├── data/                   # Dynamic content (JSON)
├── images/                 # All image assets
└── videos/                 # Video assets
```

## Root Directory

### Core Files

- **`index.html`** - Main landing page (Norwegian version)
- **`header.html`** - Shared header loaded dynamically across all pages
- **`footer.html`** - Shared footer loaded dynamically across all pages
- **`service-worker.js`** - Handles PWA caching and offline functionality
- **`manifest.webmanifest`** - Web app manifest for PWA installation
- **`robots.txt`** - Search engine crawling instructions

## Directory Structure

### `pages/` (Norwegian Content)

Contains all main content pages for the Norwegian version:

| File | Purpose |
|------|---------|
| `about.html` | "Om Oss" - Team members and information |
| `contact.html` | Contact form and contact information |
| `join.html` | Recruitment/membership page |
| `news.html` | News feed and updates |
| `rov.html` | Technical details about the ROV K2 |
| `sponsors.html` | Partners and sponsors list |
| `tac-challenge.html` | TAC Challenge information |
| `login.html` | Authentication page for internal tools |
| `equipment.html` | Internal equipment management |
| `storage.html` | Internal storage tracking |
| `instagram.html` | Instagram feed integration |

### `en/` (English Content)

Mirrors the root structure for English localization:

- `index.html` - English landing page
- `pages/` - English versions of public pages (excluding internal tools like `equipment.html`, `login.html`, `storage.html`)

### `css/` (Stylesheets)

Organized by scope and purpose:

#### Core Styles

- **`site.css`** - Main stylesheet with global styles
- **`custom.css`** - Global overrides and custom animations

#### Component Styles

- **`header.css`** - Header navigation styling
- **`footer.css`** - Footer layout and styling
- **`carousel.css`** - Image carousel component
- **`waves.css`** - Animated wave effects

#### Page-Specific Styles

- **`about.css`** - About/team page styles
- **`contact.css`** - Contact page styles
- **`equipment.css`** - Equipment management interface
- **`login.css`** - Login page styles
- **`news.css`** - News page layout
- **`rov.css`** - ROV page with 3D model viewer
- **`sponsors.css`** - Sponsors grid layout
- **`storage.css`** - Storage management interface
- **`tac-challenge.css`** - TAC Challenge page styles

### `js/` (JavaScript)

#### Core Scripts

- **`header.js`** - Dynamic header loading and auth status
- **`footer.js`** - Dynamic footer loading
- **`service-worker-register.js`** - PWA service worker registration

#### Feature Scripts

- **`members.js`** - Loads and renders member profiles from JSON
- **`github-projects.js`** - Fetches and displays GitHub repositories
- **`ui-gallery.js`** - Handles UI mockup gallery on ROV page
- **`news-render.js`** - Renders news items dynamically
- **`news.js`** - News page functionality
- **`login.js`** - Authentication logic
- **`equipment.js`** - Equipment management CRUD operations
- **`storage.js`** - Storage management functionality
- **`contact.js`** - Contact form handling
- **`sponsors.js`** - Sponsors page logic
- **`specs-collapse.js`** - Collapsible specifications logic

#### Visual Effects

- **`carousel.js`** - Image carousel functionality
- **`hero-scroll.js`** - Scroll-based animations
- **`waves.js`** - Animated wave effects
- **`front-page-video.js`** - Front page video handling
- **`embed.js`** - Embed functionality

### `data/` (Dynamic Content)

JSON files that power dynamic content:

- **`members.json`** - Team member database with profiles, roles, and groups
- *(Future)* - Additional data files for news, projects, etc.

### `images/` (Assets)

Organized by category for easy management:

#### Image Directories

- **`carousel/`** - Images for the main page slider
- **`profie_shared/`** - Team member profile pictures
- **`project/`** - Project screenshots and diagrams
- **`sponsors/`** - Sponsor logos
- **`uiasub/`** - UiASub branding, icons, and assets
  - `K2-Rov.glb` - 3D model of the K2 ROV
- **`tac-challange/`** - Images for the TAC Challenge page

## File Naming Conventions

- **HTML files**: lowercase with hyphens (e.g., `tac-challenge.html`)
- **CSS files**: match the HTML page name (e.g., `tac-challenge.css`)
- **JavaScript files**: lowercase with hyphens (e.g., `github-projects.js`)
- **Images**: lowercase with hyphens, descriptive names
- **JSON files**: lowercase with hyphens

## Localization Structure

The site supports Norwegian (default) and English:

- **Norwegian**: Root directory (`/`)
- **English**: `en/` subdirectory

Scripts automatically detect language based on:

1. URL path (`/en/` present)
2. HTML `lang` attribute
3. Default to Norwegian if neither specified

## Static Site Architecture

The site is a **static website** deployed via GitHub Pages:

- No server-side processing
- All dynamic content loaded client-side
- JSON for data storage
- Supabase Edge Functions for authenticated operations

---

[← Quick Start](Quick‐start) | [Member Data Management →](Member-Data-Management)
