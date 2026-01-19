# PWA Features

The UiASub website is a Progressive Web App (PWA), providing app-like experience with offline capabilities and installability.

## What is a PWA?

A Progressive Web App offers:

- **Installable**: Can be installed on home screen
- **Offline capable**: Works without internet connection
- **Fast**: Caches resources for quick loading
- **Engageable**: Can send push notifications (future)
- **App-like**: Feels like a native app

## Key Components

### 1. Web App Manifest (manifest.webmanifest)

Defines how the app appears when installed.

**Location**: `/manifest.webmanifest`

**Configuration**:

```json
{
  "lang": "no",
  "dir": "ltr",
  "name": "UiASub",
  "short_name": "UiASub",
  "icons": [{
    "src": "images/uiasub/Icon.png",
    "sizes": "192x192",
    "type": "image/png"
  }, {
    "src": "images/uiasub/Icon2x.png",
    "sizes": "512x512",
    "type": "image/png"
  }],
  "scope": "./",
  "id": "uiasub-app",
  "start_url": "./",
  "display": "standalone",
  "orientation": "any",
  "theme_color": "#00101a",
  "background_color": "#00101a"
}
```

**Linked in HTML**:

```html
<link rel="manifest" href="/manifest.webmanifest">
<meta name="theme-color" content="#00101a">
```

### 2. Service Worker (service-worker.js)

Handles caching and offline functionality.

**Location**: `/service-worker.js`

**Key Features**:

- Caches static assets (HTML, CSS, JS, images)
- Serves cached content when offline
- Updates cache when online
- Network-first strategy for API calls

**Implementation**:

```javascript
const CACHE_NAME = 'uiasub-v1'
const ASSETS_TO_CACHE = [
  '/',
  '/index.html',
  '/css/site.css',
  '/css/custom.css',
  '/js/header.js',
  '/js/footer.js',
  '/data/members.json',
  '/images/uiasub/logo.png'
]

// Install event - cache assets
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => {
        return cache.addAll(ASSETS_TO_CACHE)
      })
      .then(() => self.skipWaiting())
  )
})

// Activate event - clean old caches
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys()
      .then((cacheNames) => {
        return Promise.all(
          cacheNames
            .filter((name) => name !== CACHE_NAME)
            .map((name) => caches.delete(name))
        )
      })
      .then(() => self.clients.claim())
  )
})

// Fetch event - serve from cache, fallback to network
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => {
        // Cache hit - return response
        if (response) {
          return response
        }
        
        // Clone the request
        const fetchRequest = event.request.clone()
        
        return fetch(fetchRequest).then((response) => {
          // Check if valid response
          if (!response || response.status !== 200 || response.type !== 'basic') {
            return response
          }
          
          // Clone the response
          const responseToCache = response.clone()
          
          // Cache the fetched response
          caches.open(CACHE_NAME)
            .then((cache) => {
              cache.put(event.request, responseToCache)
            })
          
          return response
        })
      })
  )
})
```

### 3. Service Worker Registration (service-worker-register.js)

Registers the service worker on page load.

**Location**: `/js/service-worker-register.js`

**Implementation**:

```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js')
      .then((registration) => {
        console.log('ServiceWorker registered:', registration.scope)
        
        // Check for updates
        registration.addEventListener('updatefound', () => {
          const newWorker = registration.installing
          
          newWorker.addEventListener('statechange', () => {
            if (newWorker.state === 'installed' && navigator.serviceWorker.controller) {
              // New service worker available
              showUpdateNotification()
            }
          })
        })
      })
      .catch((error) => {
        console.error('ServiceWorker registration failed:', error)
      })
  })
  
  // Handle updates
  let refreshing = false
  navigator.serviceWorker.addEventListener('controllerchange', () => {
    if (!refreshing) {
      refreshing = true
      window.location.reload()
    }
  })
}

function showUpdateNotification() {
  const notification = document.createElement('div')
  notification.className = 'update-notification'
  notification.innerHTML = `
    <p>New version available!</p>
    <button onclick="updateApp()">Update</button>
  `
  document.body.appendChild(notification)
}

function updateApp() {
  navigator.serviceWorker.getRegistration().then((reg) => {
    reg.waiting.postMessage({ type: 'SKIP_WAITING' })
  })
}
```

## Caching Strategies

### Cache-First Strategy

Used for: Static assets (CSS, JS, images)

```javascript
caches.match(request)
  .then(response => response || fetch(request))
```

**Benefits**:

- Fastest loading
- Works offline
- Reduces bandwidth

### Network-First Strategy

Used for: Dynamic content (API calls, news)

```javascript
fetch(request)
  .catch(() => caches.match(request))
```

**Benefits**:

- Always fresh content
- Fallback to cache if offline

### Stale-While-Revalidate

Used for: Semi-dynamic content

```javascript
caches.match(request)
  .then(response => {
    const fetchPromise = fetch(request).then(networkResponse => {
      cache.put(request, networkResponse.clone())
      return networkResponse
    })
    return response || fetchPromise
  })
```

**Benefits**:

- Instant response from cache
- Updates cache in background

## Installation

### Desktop Installation

**Chrome/Edge**:

1. Visit the website
2. Look for install icon in address bar
3. Click "Install"
4. App appears in app drawer

**Install prompt** (optional):

```javascript
let deferredPrompt

window.addEventListener('beforeinstallprompt', (e) => {
  e.preventDefault()
  deferredPrompt = e
  
  // Show custom install button
  showInstallButton()
})

function showInstallButton() {
  const installBtn = document.getElementById('install-btn')
  installBtn.style.display = 'block'
  
  installBtn.addEventListener('click', async () => {
    deferredPrompt.prompt()
    const { outcome } = await deferredPrompt.userChoice
    
    if (outcome === 'accepted') {
      console.log('User accepted installation')
    }
    
    deferredPrompt = null
    installBtn.style.display = 'none'
  })
}
```

### Mobile Installation

**iOS (Safari)**:

1. Tap share button
2. Scroll and tap "Add to Home Screen"
3. Tap "Add"

**Android (Chrome)**:

1. Tap menu (three dots)
2. Tap "Add to Home Screen"
3. Tap "Add"

## Offline Functionality

### What Works Offline

- Home page
- About page (cached member data)
- ROV page
- Static content pages
- Cached images and assets

### What Requires Connection

- News feed (if not cached)
- GitHub projects
- Authentication
- Equipment management
- Contact form submission

### Offline UX

Show offline indicator:

```javascript
window.addEventListener('online', () => {
  hideOfflineNotice()
  syncPendingData()
})

window.addEventListener('offline', () => {
  showOfflineNotice()
})

function showOfflineNotice() {
  const notice = document.createElement('div')
  notice.id = 'offline-notice'
  notice.className = 'offline-notice'
  notice.innerHTML = `
    <span>You're offline</span>
  `
  document.body.appendChild(notice)
}
```

## Updates & Versioning

### Updating the Service Worker

When you update cached files:

1. **Increment cache version**:

   ```javascript
   const CACHE_NAME = 'uiasub-v2' // Was v1
   ```

2. **Update assets list** if needed:

   ```javascript
   const ASSETS_TO_CACHE = [
     // Add new files
     '/pages/new-page.html'
   ]
   ```

3. **Deploy**: Push to GitHub

4. **User update**: Automatic on next visit

### Force Update

To force immediate update:

```javascript
// In service worker
self.addEventListener('message', (event) => {
  if (event.data.type === 'SKIP_WAITING') {
    self.skipWaiting()
  }
})

// In client code
registration.waiting.postMessage({ type: 'SKIP_WAITING' })
```

## Testing PWA

### Lighthouse Audit

Run PWA audit in Chrome DevTools:

1. Open DevTools (F12)
2. Go to "Lighthouse" tab
3. Select "Progressive Web App"
4. Click "Generate report"

**Target scores**:

- PWA: 100%
- Performance: 90+
- Accessibility: 90+
- Best Practices: 90+
- SEO: 90+

### Testing Offline

**Chrome DevTools**:

1. Open DevTools (F12)
2. Go to "Network" tab
3. Check "Offline" checkbox
4. Reload page

**Service Worker inspection**:

1. DevTools → Application tab
2. Service Workers section
3. View status, update, unregister

## Troubleshooting

### Service Worker not registering

**Check**:

- HTTPS enabled (or localhost)
- Service worker file at root
- No JavaScript errors
- Browser supports service workers

### Cache not updating

**Solution**:

```javascript
// Unregister old service worker
navigator.serviceWorker.getRegistrations()
  .then(registrations => {
    registrations.forEach(reg => reg.unregister())
  })

// Clear caches
caches.keys().then(names => {
  names.forEach(name => caches.delete(name))
})

// Hard refresh
location.reload(true)
```

### App not installable

**Requirements**:

- Valid manifest.webmanifest
- Service worker registered
- HTTPS enabled
- Icons provided (192px, 512px)
- start_url resolves
- No console errors

## Best Practices

1. **Keep service worker simple**: Complex logic can cause issues
2. **Version your caches**: Always increment version on changes
3. **Cache selectively**: Don't cache everything
4. **Handle errors gracefully**: Provide fallback content
5. **Test offline thoroughly**: Simulate various offline scenarios
6. **Update responsibly**: Don't break existing installations

---

[← JavaScript & Logic](JavaScript-Logic) | [Analytics →](Analytics)
