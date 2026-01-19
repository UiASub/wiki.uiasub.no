# News & Updates Management

Guide to managing news items and announcements on the UiASub website.

## Implementation Overview

The news page (`pages/news.html`) loads content dynamically from a JSON file (`data/news.json`). This allows for easy updates without touching the HTML code.

## Updating News & Instagram

All data is stored in `data/news.json`.

### File Structure

```json
{
  "news": [
    {
      "title": "News Title (NO)",
      "title_en": "News Title (EN)",
      "description": "Short description (NO)...",
      "description_en": "Short description (EN)...",
      "image": "/images/news/your-image.png",
      "link": "https://example.com",
      "date": "10. okt 2025",
      "date_en": "10 Oct 2025",
      "badge": "Media",
      "badge_en": "Media",
      "badgeType": "media"
    }
  ],
  "instagram": [
    "https://www.instagram.com/p/DFSUDk1oeTM/",
    "https://www.instagram.com/p/DIraYgvNJ_U/"
  ]
}
```

### Adding a News Item

1.  **Prepare your image**:
    *   Save your image to `images/news/`.
    *   Recommended size: ~800x600px or similar aspect ratio.
    *   Format: PNG or JPG.

2.  **Edit `data/news.json`**:
    *   Add a new object to the `news` array.
    *   **Fields**:
        *   `title` / `title_en`: The headline (Norwegian/English).
        *   `description` / `description_en`: A brief summary (Norwegian/English).
        *   `image`: Path to the image (e.g., `/images/news/my-image.png`).
        *   `link`: URL to the full article or page.
        *   `date` / `date_en`: Date string (e.g., "10. okt 2025" / "10 Oct 2025").
        *   `badge` / `badge_en`: Text to display in the corner badge (e.g., "Media", "Nytt", "Fokus").
        *   `badgeType`: Determines the icon and style.
            *   `"media"`: Gray badge with document icon.
            *   `"new"`: Blue badge with newspaper icon.
            *   `"info"`: Blue badge (default style).

### Adding an Instagram Post

1.  **Get the Link**:
    *   Go to the Instagram post.
    *   Copy the URL (e.g., `https://www.instagram.com/p/CODE/`).

2.  **Edit `data/news.json`**:
    *   Add the URL string to the `instagram` array.
    *   **Note**: Just the URL is needed. The website will automatically generate the embed code.

### Example

```json
"instagram": [
  "https://www.instagram.com/p/DFSUDk1oeTM/",
  "https://www.instagram.com/p/NEW_POST_ID/"
]
```

## Technical Details

-   **`js/news.js`**: Fetches the JSON data and renders the HTML.
-   **`pages/news.html`**: Contains the container elements `#news-container` and `#instagram-container`.
-   **Instagram Embeds**: The script automatically adds the necessary Instagram embed boilerplate code and triggers `instgrm.Embeds.process()` to render the widgets.

---

[← Project Structure](Project-Structure) | [Sponsors Management →](Sponsors-Management)
