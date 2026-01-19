# Quick Start Guide

Get the UiASub website running locally in just a few minutes!

## Prerequisites

- **Git** - For cloning the repository
- **A local web server** - To avoid CORS issues with JSON data
- **A code editor** - VS Code recommended

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/UiASub/uiasub.github.io.git
cd uiasub.github.io
```

### 2. Start a Local Server

Choose one of the following methods:

#### Option A: Python (Simplest)

```bash
python3 -m http.server
```

Then visit: `http://localhost:8000`

#### Option B: VS Code Live Server (Recommended)

1. Install the [Live Server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
2. Right-click `index.html` and select "Open with Live Server"
3. The site will open automatically in your browser

#### Option C: Node.js http-server

```bash
npx http-server
```

## Project Overview

Once running, you'll see:

- **Norwegian site** at the root (`/`)
- **English site** at `/en/`

### Key Files to Know

- `index.html` - Main landing page (Norwegian)
- `pages/` - Content pages (Norwegian)
- `en/` - English version of the site
- `data/members.json` - Team member database
- `css/` - All stylesheets
- `js/` - All JavaScript functionality

## Making Your First Change

### Update a Team Member

1. Open `data/members.json`
2. Find or add a member entry:

```json
{
  "id": 1,
  "image": "member-photo.jpg",
  "name": "Your Name",
  "role_no": "Din Rolle",
  "role_en": "Your Role",
  "group": "data"
}
```

3. Add the profile picture to `images/profie_shared/`
4. Refresh the browser to see changes

### Edit Page Content

1. Navigate to the page file (e.g., `pages/about.html`)
2. Make your HTML changes
3. Save and refresh the browser

### Modify Styles

1. For global changes: Edit `css/custom.css` or `css/site.css`
2. For page-specific changes: Edit the corresponding CSS file (e.g., `css/about.css`)
3. Changes appear immediately if using Live Server

## Testing Your Changes

Before committing:

1. Test on both Norwegian (`/`) and English (`/en/`) versions
2. Check responsive design (mobile, tablet, desktop)
3. Verify all links work
4. Test dynamic content loading (members, news, etc.)

## Deploying Changes

GitHub Pages automatically deploys when you push to the main branch:

```bash
git add .
git commit -m "Description of your changes"
git push origin main
```

Your changes will be live at [uiasub.github.io](https://uiasub.github.io) within minutes!

## Next Steps

- Read the **[Project Structure](Project‐Structure)** guide
- Learn about **[Member Data Management](Member‐Data‐Management)**
- Understand the **[Authentication System](Authentication‐System)** for internal tools
- Explore **[Styling & CSS](Styling‐CSS)** guidelines

## Troubleshooting

### JSON data not loading

- **Cause**: CORS restrictions when opening HTML files directly
- **Solution**: Always use a local web server

### Images not displaying

- **Check**: File paths are case-sensitive
- **Verify**: Images exist in the correct directory

### Live Server not working

- **Reload**: Try stopping and restarting the server
- **Check**: No other server is running on the same port

---

[← Back to Home](Home) | [Project Structure →](Project-Structure)
