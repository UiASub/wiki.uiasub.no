# Code Standards

Coding conventions and best practices for the UiASub website project.

## General Principles

1. **Consistency**: Follow established patterns in the codebase
2. **Readability**: Write code that others can easily understand
3. **Maintainability**: Make changes easy to implement
4. **Accessibility**: Ensure the site is usable by everyone
5. **Performance**: Optimize for speed and efficiency

## HTML Standards

### Semantic HTML

Use appropriate HTML5 semantic elements:

```html
<!--  Good: Semantic and accessible -->
<header>
  <nav aria-label="Main navigation">
    <ul>
      <li><a href="/">Home</a></li>
    </ul>
  </nav>
</header>

<main>
  <article>
    <h1>Page Title</h1>
    <p>Content...</p>
  </article>
</main>

<footer>
  <p>&copy; 2025 UiASub</p>
</footer>

<!-- Avoid: Non-semantic divs -->
<div class="header">
  <div class="nav">
    <div><a href="/">Home</a></div>
  </div>
</div>
```

### Accessibility

**Images**:

```html
<!--  Good: Descriptive alt text -->
<img src="rov.jpg" alt="UiASub's K2 ROV during underwater testing">

<!-- Avoid: Missing or generic alt text -->
<img src="rov.jpg">
<img src="rov.jpg" alt="image">
```

**Forms**:

```html
<!--  Good: Labeled and accessible -->
<label for="name">Name:</label>
<input type="text" id="name" name="name" required aria-required="true">

<!-- Avoid: Unlabeled inputs -->
<input type="text" placeholder="Name">
```

**Headings**:

```html
<!--  Good: Proper hierarchy -->
<h1>Main Title</h1>
  <h2>Section</h2>
    <h3>Subsection</h3>
  <h2>Another Section</h2>

<!-- Avoid: Skipping levels -->
<h1>Main Title</h1>
  <h3>Subsection</h3> <!-- Skipped h2 -->
```

### Code Formatting

- Use **2 spaces** for indentation (not tabs)
- Lowercase for element names and attributes
- Double quotes for attribute values
- Self-closing tags for void elements (optional)
- One blank line between major sections

```html
<!DOCTYPE html>
<html lang="no">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>
</head>
<body>
  <header>
    <h1>Welcome</h1>
  </header>
  
  <main>
    <section>
      <h2>Content</h2>
      <p>Text here...</p>
    </section>
  </main>
</body>
</html>
```

## CSS Standards

### Organization

Group properties by type:

```css
.element {
  /* Positioning */
  position: relative;
  top: 0;
  left: 0;
  z-index: 10;
  
  /* Display & Box Model */
  display: flex;
  flex-direction: column;
  width: 100%;
  height: auto;
  margin: var(--spacing-md);
  padding: var(--spacing-lg);
  
  /* Visual */
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-md);
  
  /* Typography */
  font-family: var(--font-family-base);
  font-size: var(--font-size-base);
  color: var(--color-text);
  
  /* Misc */
  transition: all 0.3s ease;
  cursor: pointer;
}
```

### Naming Conventions

Use **BEM** (Block Element Modifier) where appropriate:

```css
/* Block */
.card { }

/* Element */
.card__title { }
.card__content { }
.card__button { }

/* Modifier */
.card--featured { }
.card--large { }
```

**Utility classes**:

```css
/* Prefix with purpose */
.u-hidden { display: none; }
.u-sr-only { /* Screen reader only */ }
.u-text-center { text-align: center; }
```

### CSS Variables

Always use CSS variables for:

- Colors
- Spacing
- Typography
- Border radius
- Shadows

```css
/*  Good: Use variables */
.button {
  padding: var(--spacing-sm) var(--spacing-md);
  background: var(--color-primary);
  border-radius: var(--radius-md);
}

/* Avoid: Hardcoded values */
.button {
  padding: 8px 16px;
  background: #0066cc;
  border-radius: 8px;
}
```

### Responsive Design

Mobile-first approach:

```css
/* Base styles (mobile) */
.container {
  padding: var(--spacing-md);
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    padding: var(--spacing-lg);
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .container {
    padding: var(--spacing-xl);
  }
}
```

### Avoid `!important`

Only use `!important` for:

- Utility classes that must override everything
- Third-party library overrides (last resort)

```css
/*  Acceptable: Utility class */
.u-hidden {
  display: none !important;
}

/* Avoid: Regular styles */
.button {
  background: red !important; /* Bad practice */
}
```

## JavaScript Standards

### ES6+ Syntax

Use modern JavaScript features:

```javascript
//  Good: Modern syntax
const fetchData = async (url) => {
  try {
    const response = await fetch(url)
    const data = await response.json()
    return data
  } catch (error) {
    console.error('Error:', error)
    return null
  }
}

// Avoid: Old syntax
function fetchData(url) {
  var xhr = new XMLHttpRequest()
  xhr.onreadystatechange = function() {
    if (xhr.readyState === 4) {
      var data = JSON.parse(xhr.responseText)
      return data
    }
  }
  xhr.open('GET', url)
  xhr.send()
}
```

### Variable Declarations

```javascript
//  Good: const and let
const API_URL = 'https://api.example.com'
let counter = 0

// Avoid: var
var API_URL = 'https://api.example.com'
var counter = 0
```

### Functions

**Prefer arrow functions** for brevity:

```javascript
//  Good
const double = (n) => n * 2
const greet = (name) => `Hello, ${name}!`

//  Also good for complex functions
const processData = (data) => {
  const filtered = data.filter(item => item.active)
  const mapped = filtered.map(item => item.value)
  return mapped
}
```

**Use regular functions** when you need `this` context:

```javascript
class Component {
  constructor() {
    this.value = 42
  }
  
  //  Good: Regular method
  getValue() {
    return this.value
  }
}
```

### Template Literals

```javascript
//  Good: Template literals
const message = `Hello, ${name}! You have ${count} messages.`

// Avoid: String concatenation
const message = 'Hello, ' + name + '! You have ' + count + ' messages.'
```

### Error Handling

Always handle errors in async operations:

```javascript
//  Good: Proper error handling
async function loadData() {
  try {
    const response = await fetch(url)
    
    if (!response.ok) {
      throw new Error(`HTTP error ${response.status}`)
    }
    
    return await response.json()
  } catch (error) {
    console.error('Failed to load data:', error)
    showErrorMessage('Unable to load data. Please try again.')
    return []
  }
}

// Avoid: No error handling
async function loadData() {
  const response = await fetch(url)
  return await response.json()
}
```

### Comments

Write self-documenting code, but add comments for complex logic:

```javascript
//  Good: Explain why, not what
// Cache auth status for 5 minutes to reduce API calls
const CACHE_DURATION = 5 * 60 * 1000

// Normalize Discord user data to match our schema
function normalizeUser(discordUser) {
  return {
    id: discordUser.id,
    name: discordUser.username
  }
}

// Avoid: Obvious comments
// Set cache duration to 300000
const CACHE_DURATION = 300000

// Get the user's name
function getName(user) {
  return user.name
}
```

### Naming Conventions

```javascript
// Constants: UPPER_SNAKE_CASE
const MAX_RETRY_ATTEMPTS = 3
const API_BASE_URL = 'https://api.example.com'

// Variables and functions: camelCase
const userName = 'John'
function fetchUserData() { }

// Classes: PascalCase
class UserManager { }
class EquipmentController { }

// Private properties/methods: _prefixed (convention)
class MyClass {
  _privateMethod() { }
  _privateProperty = 42
}
```

## JSON Standards

### Formatting

```json
{
  "id": 1,
  "name": "John Doe",
  "role_no": "Leder",
  "role_en": "Leader",
  "group": "leadership",
  "metadata": {
    "joined": "2023-01-15",
    "active": true
  }
}
```

**Rules**:

- Use 2 spaces for indentation
- Double quotes for strings
- No trailing commas
- Use consistent key naming (snake_case for our project)

### Validation

Always validate JSON before committing:

```bash
# Python
python3 -m json.tool data/members.json

# Node.js
node -e "console.log(JSON.parse(require('fs').readFileSync('data/members.json')))"

# Online tools
# jsonlint.com
# jsonformatter.org
```

## Git Commit Standards

### Commit Message Format

```
type: Brief description (50 chars or less)

Longer explanation if needed. Wrap at 72 characters.
Explain what and why, not how.

- Bullet points are okay
- Use present tense: "Add feature" not "Added feature"
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, no logic change)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples**:

```bash
git commit -m "feat: Add dark mode toggle to header"
git commit -m "fix: Resolve navigation overflow on mobile"
git commit -m "docs: Update member data management guide"
git commit -m "style: Format CSS files with Prettier"
git commit -m "refactor: Simplify member loading logic"
```

## File Organization

### Naming

- **HTML**: lowercase-with-hyphens.html
- **CSS**: lowercase-with-hyphens.css
- **JavaScript**: lowercase-with-hyphens.js
- **Images**: lowercase-with-hyphens.jpg/png/svg
- **JSON**: lowercase-with-hyphens.json

### Directory Structure

Keep related files together:

```bash
css/
  components/
    header.css
    footer.css
  pages/
    about.css
    rov.css
  core/
    site.css
    variables.css
```

## Performance Best Practices

### Images

- Optimize before committing (< 200KB for photos)
- Use appropriate formats:
  - Photos: JPG
  - Graphics/logos: PNG or SVG
  - Icons: SVG preferred
- Provide alt text
- Use lazy loading for below-fold images

### CSS

- Minimize use of expensive properties (`box-shadow`, `filter`)
- Use `transform` and `opacity` for animations
- Avoid deep nesting (max 3-4 levels)
- Remove unused CSS

### JavaScript

- Avoid global variables
- Debounce expensive operations
- Use event delegation for dynamic content
- Lazy load non-critical scripts

## Accessibility Checklist

- [ ] All images have descriptive alt text
- [ ] Heading hierarchy is correct (h1 → h2 → h3)
- [ ] Color contrast meets WCAG AA standards (4.5:1)
- [ ] Forms have proper labels
- [ ] Interactive elements are keyboard accessible
- [ ] ARIA attributes used where appropriate
- [ ] Focus indicators are visible
- [ ] Content is readable at 200% zoom

## Browser Support

Target:

- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)
- Mobile browsers (iOS Safari, Chrome Mobile)

Don't support:

- Internet Explorer

## Testing Checklist

Before pushing:

- [ ] Code works on both Norwegian and English versions
- [ ] Responsive design tested (mobile, tablet, desktop)
- [ ] No console errors
- [ ] Images load correctly
- [ ] Links work
- [ ] Forms submit properly (if applicable)
- [ ] Tested on multiple browsers
- [ ] Accessibility validated

---

[← Contributing](Contributing) | [Back to Home](Home)
