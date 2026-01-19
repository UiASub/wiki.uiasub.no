# Member Data Management

The "About Us" page dynamically loads team member profiles from a JSON database, making it easy to manage the team roster without touching HTML code.

## Overview

Member data is stored in `data/members.json` and rendered by `js/members.js`. The system automatically:

- Groups members by their team/section
- Supports bilingual display (Norwegian/English)
- Handles profile images
- Adapts to language context

## File Locations

| Resource | Path |
|----------|------|
| **Data File** | `data/members.json` |
| **Profile Images** | `images/profie_shared/` |
| **Rendering Script** | `js/members.js` |
| **Display Page (NO)** | `pages/about.html` |
| **Display Page (EN)** | `en/pages/about.html` |

## Member Object Structure

Each member in the JSON array has the following fields:

```json
{
  "id": 12,
  "image": "member-photo.jpg",
  "name": "Ola Nordmann",
  "role_no": "Leder",
  "role_en": "Leader",
  "group": "leadership",
  "_comment": "Optional internal note"
}
```

### Field Descriptions

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | Number | | Unique identifier for the member |
| `image` | String | | Filename of profile picture in `images/profie_shared/` |
| `name` | String | | Full name (use "Medlem" for generic placeholders) |
| `role_no` | String | | Role/title in Norwegian |
| `role_en` | String | | Role/title in English |
| `group` | String | | Section ID (see Groups below) |

## Groups (Team Sections)

Members are automatically organized into sections based on their `group` field:

| Group ID | Norwegian Name | English Name |
|----------|----------------|--------------|
| `leadership` | Ledelse | Leadership |
| `data` | Data | Data |
| `electronics` | Elektronikk | Electronics |
| `mechanical` | Mekanisk | Mechanical |
| `marketing` | Markedsføring | Marketing |
| `members` | Medlemmer | Members |

### Group Display Order

The order members appear is determined by two factors:

1. **Group order** - Members are displayed in groups in this specific order (lines 9-16):
   - Leadership
   - Data
   - Electronics
   - Mechanical
   - Marketing
   - Members

2. **Within each group** - Members appear in the same order as they appear in the `members.json` file (line 22: `members.filter(m => m.group === group.id)` preserves the original array order).

So if you want to change the order of members within a group, simply reorder them in the `members.json` file. The first person with `"group": "leadership"` will appear first in the Leadership section, the second will appear second, etc.

## Adding a New Member

### Step 1: Prepare the Profile Picture

1. Save the image to `images/profie_shared/`
2. Use a clear, professional photo
3. Recommended: Square aspect ratio (1:1)
4. File format: `.jpg`, `.png`, or `.webp`
5. File naming: lowercase with hyphens (e.g., `ola-nordmann.jpg`)

### Step 2: Add to JSON

Open `data/members.json` and add a new object:

```json
{
  "id": 25,
  "image": "ola-nordmann.jpg",
  "name": "Ola Nordmann",
  "role_no": "Dataansvarlig",
  "role_en": "Data Lead",
  "group": "data"
}
```

**Important**: Assign a unique `id` (use the next available number).

### Step 3: Verify

1. Save the file
2. Refresh the website
3. Check both Norwegian and English versions
4. Verify the member appears in the correct section

## Editing a Member

1. Open `data/members.json`
2. Find the member by `id` or `name`
3. Update the desired fields
4. Save and refresh the site

**Example**: Changing a member's role:

```json
{
  "id": 12,
  "image": "kari-nordmann.jpg",
  "name": "Kari Nordmann",
  "role_no": "Nestleder",  // Changed from "Medlem"
  "role_en": "Vice Leader", // Changed from "Member"
  "group": "leadership"     // Changed from "members"
}
```

## Removing a Member

### Option 1: Delete the Entry (Permanent)

Remove the entire JSON object from the array.

### Option 2: Archive with Comment (Recommended)

Keep the entry but add a comment for historical records:

```json
{
  "id": 15,
  "image": "former-member.jpg",
  "name": "Former Member",
  "role_no": "Tidligere medlem",
  "role_en": "Former Member",
  "group": "members",
  "_comment": "ARCHIVED - Left team in 2024"
}
```

Then manually remove it later or filter it programmatically.

## Placeholder Members

For recruitment or when positions are open:

```json
{
  "id": 30,
  "image": "placeholder.jpg",
  "name": "Medlem",
  "role_no": "Søker ny medlem",
  "role_en": "Seeking new member",
  "group": "electronics"
}
```

Use a generic placeholder image and "Medlem" as the name.

## Troubleshooting

### Member not appearing

**Check**:

1. JSON syntax is valid (no missing commas or brackets)
2. `group` value matches one of the valid groups
3. Image file exists in `images/profie_shared/`
4. Image filename matches exactly (case-sensitive)
5. Browser cache cleared (Ctrl+Shift+R / Cmd+Shift+R)

### Image not loading

**Check**:

1. File path is correct
2. File extension matches (`.jpg` vs `.jpeg`)
3. No spaces in filename
4. Image file is not corrupted

### Wrong language displayed

The `members.js` script detects language automatically:

- Checks URL for `/en/`
- Falls back to HTML `lang` attribute
- Displays `role_en` for English, `role_no` for Norwegian

### JSON validation error

Use a JSON validator:

```bash
# Using Python
python3 -m json.tool data/members.json

# Using Node.js
node -e "JSON.parse(require('fs').readFileSync('data/members.json'))"
```

## Best Practices

1. **Consistent IDs**: Always use sequential, unique IDs
2. **Image Quality**: Use optimized images (compress before uploading)
3. **Naming Convention**: Use lowercase with hyphens for all filenames
4. **Commit Messages**: Be descriptive when updating members

   ```bash
   git commit -m "Add new data team member: Ola Nordmann"
   ```

5. **Test Both Languages**: Always verify changes in both NO and EN versions

---

[← Project Structure](Project-Structure) | [Authentication System →](Authentication-System)
