# Sponsors Management

Guide to managing sponsor information and logos on the UiASub website.

## Overview

Sponsors are displayed on the sponsors page with logos, descriptions, and links:

- `pages/sponsors.html` (Norwegian)
- `en/pages/sponsors.html` (English)

## Current Structure

All sponsors are displayed in a rotating carousel on the sponsors page, ensuring equal visibility for all partners.

## Adding a New Sponsor

1.  **Prepare the Logo**:
    *   Save your image to `images/sponsors/`.
    *   Format: PNG or WebP.

2.  **Edit `data/sponsors.json`**:
    *   Add a new object to the array.
    *   **Fields**:
        *   `name`: Name of the sponsor.
        *   `image`: Path to the logo (e.g., `/images/sponsors/logo.png`).
        *   `description`: Description in Norwegian.
        *   `description_en`: Description in English.
        *   `link`: Website URL.

### Example

```json
{
  "name": "New Sponsor",
  "image": "/images/sponsors/new-logo.png",
  "description": "Beskrivelse på norsk.",
  "description_en": "Description in English.",
  "link": "https://example.com"
}
```

## Technical Details

-   **`js/sponsors.js`**: Fetches `data/sponsors.json` and populates the Owl Carousel.
-   **`pages/sponsors.html`**: Contains the empty `.owl-carousel` container.


## Removing a Sponsor

1.  **Edit `data/sponsors.json`**:
    *   Remove the object corresponding to the sponsor you want to delete.

## Logo Guidelines

### Quality Standards

- **Resolution**: At least 2x display size (for retina screens)
- **Format**:
  - SVG preferred (scales perfectly)
  - PNG with transparency second choice
  - Avoid JPG (no transparency, compression artifacts)
- **Color**:
  - Original brand colors
  - Ensure good contrast on white background

### Optimization Checklist

Before adding a logo:

- [ ] Correct format (SVG or PNG)
- [ ] Transparent background (if PNG)
- [ ] Optimized file size (< 100KB)
- [ ] Proper dimensions (400-800px width)

---

[← News & Updates](News-Updates) | [Back to Home](Home)
