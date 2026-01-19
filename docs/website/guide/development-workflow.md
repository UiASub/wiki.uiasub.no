# Development Workflow

Best practices and workflows for developing and maintaining the UiASub website.

## Git Workflow

### Branch Strategy

**Main Branch**: `main` (or `master`)

- Always deployable
- Protected (requires pull requests)
- Automatically deploys to GitHub Pages

**Feature Branches**: `feature/feature-name`

- Created from `main`
- One feature per branch
- Deleted after merge

### Making Changes

#### 1. Create a Feature Branch

```bash
git checkout main
git pull origin main
git checkout -b feature/add-new-member
```

#### 2. Make Your Changes

Edit files as needed, test locally.

#### 3. Commit Changes

```bash
git add .
git commit -m "Add new data team member"
```

**Commit Message Guidelines**:

- Use present tense ("Add feature" not "Added feature")
- Be descriptive but concise
- Reference issue numbers if applicable

Examples:

```bash
git commit -m "Add new member to data team"
git commit -m "Fix mobile navigation overflow"
git commit -m "Update sponsor logos for 2025"
git commit -m "Improve equipment table accessibility"
```

#### 4. Push to GitHub

```bash
git push origin feature/add-new-member
```

#### 5. Create Pull Request

1. Go to GitHub repository
2. Click "New Pull Request"
3. Select your feature branch
4. Add description of changes
5. Request review (if applicable)
6. Wait for CI checks to pass

#### 6. Merge and Deploy

1. Merge pull request on GitHub
2. Delete feature branch
3. GitHub Pages automatically deploys

### Hotfixes

For urgent fixes to production:

```bash
git checkout main
git pull origin main
git checkout -b hotfix/critical-bug
# Make fix
git commit -m "Fix critical navigation bug"
git push origin hotfix/critical-bug
# Create PR and merge immediately
```

## Local Development

### Setup

```bash
# Clone repository
git clone https://github.com/UiASub/uiasub.github.io.git
cd uiasub.github.io

# Start local server
python3 -m http.server
# or
npx http-server
```

### Live Reload (Recommended)

Use VS Code Live Server extension for automatic reload:

1. Install "Live Server" extension
2. Right-click `index.html`
3. Select "Open with Live Server"
4. Changes appear instantly

### Testing Changes

Before committing, test:

#### Both Languages

- Norwegian version (`/`)
- English version (`/en/`)

#### Multiple Pages

- Home page
- Pages affected by your changes
- Header and footer on all pages

#### Responsive Design

Test on different screen sizes:

- Mobile (320px - 480px)
- Tablet (768px - 1024px)
- Desktop (1280px+)

**Browser DevTools**: Press `F12` → Toggle device toolbar

#### Cross-Browser Compatibility

Test on:

- Chrome/Edge (Chromium)
- Firefox
- Safari (if on macOS)

#### Dynamic Content

- Member profiles loading
- News items rendering
- GitHub projects fetching
- Authentication flow (if applicable)

## Code Quality

### HTML Standards

- Use semantic HTML5 elements
- Include `alt` text for all images
- Maintain proper heading hierarchy (`h1` → `h2` → `h3`)
- Close all tags properly
- Use lowercase for element names and attributes

**Example**:

```html
<!-- Good -->
<section class="team-section">
  <h2>Our Team</h2>
  <img src="team.jpg" alt="UiASub team photo 2025">
</section>

<!-- Avoid -->
<div class="team-section">
  <h3>Our Team</h3>
  <img src="team.jpg">
</div>
```

### CSS Standards

- Use CSS custom properties for theming
- Follow BEM naming convention where applicable
- Mobile-first responsive design
- Minimize use of `!important`
- Group related properties

**Example**:

```css
/* Good */
.card {
  padding: var(--spacing-md);
  background: var(--color-surface);
  border-radius: var(--radius-lg);
}

.card__title {
  font-size: var(--font-size-lg);
  color: var(--color-primary);
}

/* Avoid */
.card {
  padding: 20px !important;
  background: #ffffff;
}

.cardTitle {
  font-size: 24px;
}
```

### JavaScript Standards

- Use `const` and `let` (not `var`)
- Use template literals for strings
- Handle errors gracefully
- Add comments for complex logic
- Use async/await for promises

**Example**:

```javascript
// Good
const fetchMembers = async () => {
  try {
    const response = await fetch('/data/members.json')
    const members = await response.json()
    return members
  } catch (error) {
    console.error('Failed to load members:', error)
    return []
  }
}

// Avoid
function fetchMembers() {
  var xhr = new XMLHttpRequest()
  xhr.open('GET', '/data/members.json')
  xhr.send()
  // No error handling
}
```

### JSON Standards

- Validate JSON before committing
- Use consistent indentation (2 spaces)
- No trailing commas
- Use double quotes for strings

**Validation**:

```bash
# Python
python3 -m json.tool data/members.json

# Node.js
node -e "JSON.parse(require('fs').readFileSync('data/members.json'))"

# Online: jsonlint.com
```

## Common Tasks

### Adding a New Page

1. **Create HTML file**:

   ```bash
   # Norwegian version
   touch pages/new-page.html
   
   # English version
   touch en/pages/new-page.html
   ```

2. **Add page-specific CSS** (if needed):

   ```bash
   touch css/new-page.css
   ```

3. **Update navigation**:
   - Edit `header.html` to add link
   - Update both Norwegian and English versions

4. **Test and commit**:

   ```bash
   git add pages/new-page.html en/pages/new-page.html css/new-page.css header.html
   git commit -m "Add new page for [purpose]"
   ```

### Updating Member Data

See [Member Data Management](Member‐Data‐Management) for detailed guide.

**Quick version**:

```bash
# 1. Add profile picture
cp new-member.jpg images/profie_shared/

# 2. Edit JSON
nano data/members.json

# 3. Commit
git add images/profie_shared/new-member.jpg data/members.json
git commit -m "Add new member: [Name]"
git push
```

### Updating Sponsors

1. **Add logo**:

   ```bash
   cp sponsor-logo.png images/sponsors/
   ```

2. **Edit sponsor page**:
   - Update `pages/sponsors.html` (Norwegian)
   - Update `en/pages/sponsors.html` (English)

3. **Optimize image**:

   ```bash
   # Use ImageOptim, TinyPNG, or similar
   ```

4. **Commit**:

   ```bash
   git add images/sponsors/sponsor-logo.png pages/sponsors.html en/pages/sponsors.html
   git commit -m "Add new sponsor: [Company Name]"
   git push
   ```

### Adding News Items

News items are typically hardcoded in HTML. To make it dynamic (recommended future improvement):

**Current approach**: Edit `pages/news.html` directly

**Future approach**: Use JSON similar to members:

1. Create `data/news.json`
2. Add rendering script `js/news-render.js`
3. Update pages to load dynamically

## Performance Optimization

### Image Optimization

Always optimize images before committing:

**Tools**:

- [TinyPNG](https://tinypng.com/) - Online compression
- [ImageOptim](https://imageoptim.com/) - macOS app
- [Squoosh](https://squoosh.app/) - Web-based

**Guidelines**:

- Profile photos: Max 500KB, 800x800px
- Hero images: Max 200KB, 1920px width
- Logos: Use SVG when possible, or PNG with transparency
- Icons: Use SVG or icon fonts

### Code Minification

For production (optional):

```bash
# CSS
npx clean-css-cli -o style.min.css style.css

# JavaScript
npx terser script.js -o script.min.js
```

**Note**: Currently not implemented. Consider for future optimization.

## Deployment

### Automatic Deployment

GitHub Pages automatically deploys when changes are pushed to `main`:

1. **Push to main** → Triggers GitHub Actions
2. **Build** → Jekyll processes files (minimal for static site)
3. **Deploy** → Updates live site
4. **Live** → Changes visible at [uiasub.github.io](https://uiasub.github.io)

**Deployment time**: ~1-3 minutes

### Manual Testing Before Merge

1. Create PR
2. Use GitHub Pages preview (if configured)
3. Or test locally and trust the process

### Rollback

If a deployment breaks the site:

```bash
# Find the last working commit
git log --oneline

# Revert to that commit
git revert <commit-hash>
git push origin main
```

Or use GitHub's "Revert" button on the merged PR.

## Troubleshooting

### Changes not appearing on live site

1. **Wait**: Deployment takes 1-3 minutes
2. **Clear cache**: Hard refresh (Ctrl+Shift+R / Cmd+Shift+R)
3. **Check GitHub Actions**: Verify deployment succeeded
4. **Verify merge**: Ensure PR was actually merged

### Local server CORS errors

**Cause**: Opening HTML files directly (`file://`)

**Solution**: Always use a local web server:

```bash
python3 -m http.server
```

### Git conflicts

```bash
# Update your branch with latest main
git checkout main
git pull origin main
git checkout your-feature-branch
git merge main

# Resolve conflicts in editor
# Then commit the merge
git add .
git commit -m "Merge main into feature branch"
```

---

[← Authentication System](Authentication-System) | [Contributing →](Contributing)
