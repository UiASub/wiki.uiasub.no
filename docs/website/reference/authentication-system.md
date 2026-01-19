# Authentication System

The UiASub website includes an authentication system for internal tools, using Discord OAuth via Supabase for secure, role-based access control.

## Overview

Internal tools like equipment management and storage tracking require authentication. The system leverages:

- **Discord OAuth 2.0** for identity verification
- **Supabase** for backend authentication and database
- **Supabase Edge Functions** for serverless role verification
- **localStorage** for session persistence

## Architecture

```
User → Login Page → Discord OAuth → Supabase Auth
                                       ↓
                            Supabase Edge Function
                                       ↓
                          Discord API (Role Check)
                                       ↓
                              Return Token + Status
                                       ↓
                         Client stores token in localStorage
```

## Authentication Flow

### 1. User Initiates Login

**Trigger**: User clicks "Logg Inn" / "Log In" in the header

**Action**: Redirects to `pages/login.html`

### 2. Discord OAuth Redirect

**Page**: `pages/login.html`

**Process**:

- User clicks the Discord login button
- Supabase redirects to Discord OAuth
- User authorizes the application
- Discord redirects back to the site

### 3. Token Exchange & Verification

**Script**: `js/login.js`

**Process**:

```javascript
// 1. Extract access token from URL
const token = supabase.auth.getSession()

// 2. Send token to Edge Function for role verification
const response = await fetch(edgeFunctionURL, {
  headers: { Authorization: `Bearer ${token}` }
})

// 3. Edge Function checks Discord role
const hasRequiredRole = await checkDiscordRole(userId)

// 4. Return verification result
return { verified: hasRequiredRole, user: userData }
```

### 4. Session Storage

**If verified**:

- Token stored in `localStorage.setItem('sb-auth-token', token)`
- User redirected to requested internal page
- Session persists across page loads

**If not verified**:

- Error message displayed
- User remains on login page
- No token stored

### 5. Session Validation

**On every page load** (`js/header.js`):

```javascript
// Check if token exists
const token = localStorage.getItem('sb-auth-token')

if (token) {
  // Verify token is still valid (cached for 5 minutes)
  const isValid = await verifyToken(token)
  
  if (isValid) {
    // Show "Logged In" state
  } else {
    // Clear invalid token and show "Log In"
    localStorage.removeItem('sb-auth-token')
  }
}
```

## Key Files

### Frontend

| File | Purpose |
|------|---------|
| `pages/login.html` | Login page with Discord OAuth button |
| `js/login.js` | Handles OAuth flow and token verification |
| `js/header.js` | Checks auth status and updates UI |
| `js/equipment.js` | Authenticated equipment management |
| `js/storage.js` | Authenticated storage management |

### Backend (Supabase)

| Resource | Purpose |
|----------|---------|
| **Edge Function**: `discord-role-sync` | Verifies Discord roles server-side |
| **Database**: `equipment` table | Stores equipment data |
| **Database**: `storage` table | Stores storage data |
| **Auth**: Discord OAuth provider | Handles user authentication |

## Required Discord Roles

The Edge Function checks for specific Discord roles:

| Role Name | Access Level |
|-----------|--------------|
| `Member` | Read-only access to internal tools |
| `Leadership` | Full CRUD access to internal tools |

**Note**: Role names are configurable in the Edge Function.

## Supabase Edge Function

### Purpose

Server-side role verification to prevent client-side bypass.

### Endpoint

```
https://[project-id].supabase.co/functions/v1/discord-role-sync
```

### Authentication

```javascript
fetch(endpoint, {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${accessToken}`,
    'Content-Type': 'application/json'
  }
})
```

### Response

```json
{
  "verified": true,
  "user": {
    "id": "discord-user-id",
    "username": "username#0000",
    "roles": ["Member", "Leadership"]
  }
}
```

## Protected Pages

### Equipment Management (`pages/equipment.html`)

**Features**:

- View equipment inventory
- Add new equipment
- Assign equipment to members
- Return equipment
- Delete equipment records

**Access Control**:

- Read: All authenticated members
- Write: Leadership only

### Storage Management (`pages/storage.html`)

**Features**:

- View storage locations
- Add storage items
- Update item status
- Remove items

**Access Control**:

- Read: All authenticated members
- Write: Leadership only

## Security Measures

### Client-Side

1. **Token Storage**: `localStorage` (secure, but not HTTP-only)
2. **Token Validation**: Verified on every protected page load
3. **Timeout**: Tokens expire after session timeout
4. **Cache**: Auth status cached for 5 minutes to reduce API calls

### Server-Side

1. **Role Verification**: All operations verified via Edge Function
2. **JWT Validation**: Supabase validates JWT tokens
3. **Discord API**: Roles fetched directly from Discord (cannot be spoofed)
4. **Row-Level Security**: Supabase RLS policies enforce permissions

## Session Management

### Login

```javascript
// User logs in successfully
localStorage.setItem('sb-auth-token', token)
localStorage.setItem('sb-user-data', JSON.stringify(userData))
```

### Logout

```javascript
// Clear session
localStorage.removeItem('sb-auth-token')
localStorage.removeItem('sb-user-data')
supabase.auth.signOut()
```

### Auto-Expiry

- Tokens expire after 1 hour (Supabase default)
- Refresh token used to renew session automatically
- Invalid tokens cleared automatically

## Header Integration

The header (`header.html` + `js/header.js`) dynamically updates based on auth status:

### Not Logged In

```html
<a href="/pages/login.html">Logg Inn</a>
```

### Logged In

```html
<span>Logged In</span>
<a href="#" onclick="logout()">Logg Ut</a>
```

## Troubleshooting

### "Not authorized" error

**Cause**: User doesn't have required Discord role

**Solution**:

1. Verify Discord server membership
2. Check role assignment in Discord
3. Contact admin to assign correct role

### Token expired

**Cause**: Session exceeded timeout period

**Solution**: Log in again (tokens auto-refresh if refresh token is valid)

### Can't access internal pages

**Checklist**:

1. Logged in via Discord
2. Token exists in localStorage
3. Required Discord role assigned
4. Edge Function is running
5. Not using private/incognito mode (clears localStorage)

### Role verification failing

**Debug**:

```javascript
// Check stored token
console.log(localStorage.getItem('sb-auth-token'))

// Test Edge Function manually
fetch('https://[project-id].supabase.co/functions/v1/discord-role-sync', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  }
})
.then(res => res.json())
.then(console.log)
```

## Setup for Developers

### Prerequisites

1. Supabase project created
2. Discord OAuth app configured
3. Edge Function deployed

### Configuration Steps

1. **Configure Discord OAuth in Supabase**:
   - Dashboard → Authentication → Providers → Discord
   - Add Discord Client ID and Secret
   - Set redirect URL: `https://[project-id].supabase.co/auth/v1/callback`

2. **Deploy Edge Function**:

   ```bash
   supabase functions deploy discord-role-sync
   ```

3. **Set Environment Variables**:

   ```bash
   supabase secrets set DISCORD_BOT_TOKEN=your_bot_token
   supabase secrets set DISCORD_GUILD_ID=your_server_id
   ```

4. **Update Client Configuration**:

   ```javascript
   // js/login.js
   const SUPABASE_URL = 'https://[project-id].supabase.co'
   const SUPABASE_ANON_KEY = 'your_anon_key'
   ```

---

[← Member Data Management](Member-Data-Management) | [Styling & CSS →](Styling-CSS)
