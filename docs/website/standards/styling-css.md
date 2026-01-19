# Styling & CSS

Guide to the CSS architecture, styling conventions, and customization for the UiASub website.

## CSS Architecture

The website uses a layered CSS architecture combining Bootstrap 5, Tailwind CSS utilities, and custom styles.

### CSS Stack

| Layer | Purpose | Files |
|-------|---------|-------|
| **Framework** | Base responsive grid and components | Bootstrap 5 (CDN) |
| **Utilities** | Utility-first classes | Tailwind CSS (CDN) |
| **Core** | Site-wide base styles | `css/site.css` |
| **Custom** | Global custom styles and animations | `css/custom.css` |
| **Components** | Reusable component styles | `css/header.css`, `css/footer.css`, etc. |
| **Pages** | Page-specific styles | `css/about.css`, `css/rov.css`, etc. |

### Load Order

```html
<!-- 1. Framework -->
<link href="bootstrap.min.css" rel="stylesheet">
<link href="tailwind.min.css" rel="stylesheet">

<!-- 2. Core -->
<link href="/css/site.css" rel="stylesheet">

<!-- 3. Components -->
<link href="/css/header.css" rel="stylesheet">
<link href="/css/footer.css" rel="stylesheet">

<!-- 4. Page-specific -->
<link href="/css/about.css" rel="stylesheet">

<!-- 5. Custom overrides -->
<link href="/css/custom.css" rel="stylesheet">
```

## Core Styles (site.css)

### Base Resets

```css
body {
  padding-top: 50px;
}

.nossheader-logo {
  padding-top: 12px;
  padding-left: 12px;
}
```

### Typography Defaults

```css
h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-family-heading);
  font-weight: var(--font-weight-bold);
  line-height: 1.2;
  margin-top: 0;
}

a {
  color: var(--color-primary);
  text-decoration: none;
  transition: color 0.2s;
}

a:hover {
  color: var(--color-primary-dark);
}
```

## Custom Styles (custom.css)

```css
html {
  background: #000000;
}

/* Sea-like background for the whole site */
body {
  background: linear-gradient(180deg, var(--sea-top) 0%, var(--sea-deep) 60%);
  color: #e6f7ff;
}
```

Includes styles for:
- Callouts
- Marketing sections
- Featurettes
- Powered-by container
- Sidebar


### Wave Animation

Located in `css/waves.css`:

```css
.waves {
  position: relative;
  width: 100%;
  height: 15vh;
  margin-bottom: -7px;
  min-height: 100px;
  max-height: 150px;
}

.wave {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100px;
  background: url('wave.svg');
  animation: wave 10s linear infinite;
}

@keyframes wave {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}
```

## Component Styles

### Header (header.css)

```css
.site-header {
  background: var(--color-primary);
  box-shadow: var(--shadow-md);
  position: sticky;
  top: 0;
  z-index: 1000;
}

.site-nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: var(--spacing-md) var(--spacing-lg);
}

.site-nav a {
  color: white;
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: var(--radius-md);
  transition: background 0.2s;
}

.site-nav a:hover {
  background: var(--color-primary-dark);
}
```

### Carousel (carousel.css)

```css
.carousel-container {
  position: relative;
  width: 100%;
  height: 500px;
  overflow: hidden;
}

.carousel-slide {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease-in-out;
}

.carousel-slide.active {
  opacity: 1;
}
```

## Page-Specific Styles

### About Page (about.css)

```css
.team-section {
  padding: var(--spacing-3xl) 0;
}

.team-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: var(--spacing-lg);
}

.member-card {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  padding: var(--spacing-lg);
  text-align: center;
  box-shadow: var(--shadow-md);
  transition: transform 0.2s, box-shadow 0.2s;
}

.member-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-lg);
}

.member-image {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: var(--spacing-md);
}
```

### ROV Page (rov.css)

```css
.rov-viewer {
  width: 100%;
  height: 600px;
  position: relative;
  background: linear-gradient(to bottom, #001f3f, #003366);
}

.model-canvas {
  width: 100%;
  height: 100%;
}

.rov-controls {
  position: absolute;
  bottom: var(--spacing-lg);
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: var(--spacing-sm);
}
```

## Responsive Design

### Breakpoints

```css
/* Mobile first approach */
:root {
  --breakpoint-sm: 576px;   /* Small devices */
  --breakpoint-md: 768px;   /* Tablets */
  --breakpoint-lg: 992px;   /* Desktops */
  --breakpoint-xl: 1200px;  /* Large desktops */
  --breakpoint-2xl: 1400px; /* Extra large */
}
```

### Media Queries

```css
/* Mobile: Base styles (default) */
.container {
  padding: var(--spacing-md);
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    padding: var(--spacing-lg);
  }
  
  .team-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop and up */
@media (min-width: 992px) {
  .container {
    padding: var(--spacing-xl);
  }
  
  .team-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Large desktop */
@media (min-width: 1200px) {
  .team-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

## Customization Guide

### Changing the Color Scheme

1. Open `css/custom.css` or `css/site.css`
2. Update the color variables:

```css
:root {
  --color-primary: #your-new-color;
  --color-primary-dark: #darker-shade;
  --color-primary-light: #lighter-shade;
}
```

3. Save and refresh

### Adding a Custom Animation

1. Open `css/custom.css`
2. Add your keyframes:

```css
@keyframes your-animation {
  0% { /* start state */ }
  100% { /* end state */ }
}

.your-class {
  animation: your-animation 2s ease-in-out infinite;
}
```

### Creating Page-Specific Styles

1. Create new CSS file:

   ```bash
   touch css/your-page.css
   ```

2. Add styles:

   ```css
   .your-page-class {
     /* Your styles */
   }
   ```

3. Link in HTML:

   ```html
   <link href="/css/your-page.css" rel="stylesheet">
   ```

## Best Practices

### CSS Organization

1. **Group related properties**:

   ```css
   .button {
     /* Positioning */
     display: inline-block;
     position: relative;
     
     /* Box model */
     padding: var(--spacing-sm) var(--spacing-md);
     margin: var(--spacing-xs);
     
     /* Visual */
     background: var(--color-primary);
     border-radius: var(--radius-md);
     
     /* Typography */
     color: white;
     font-weight: var(--font-weight-medium);
     
     /* Misc */
     transition: all 0.2s;
     cursor: pointer;
   }
   ```

2. **Use CSS variables**: Always prefer variables over hardcoded values

3. **Mobile-first**: Write base styles for mobile, then enhance for larger screens

4. **Avoid !important**: Only use when absolutely necessary (e.g., utility classes)

5. **Consistent naming**: Use BEM or similar methodology

### Performance

1. **Minimize repaints**: Animate `transform` and `opacity` instead of `top`/`left`
2. **Use `will-change`**: For frequently animated elements
3. **Lazy load animations**: Don't animate off-screen elements
4. **Optimize selectors**: Avoid overly complex selectors

---

[← Authentication System](Authentication-System) | [JavaScript & Logic →](JavaScript-Logic)
