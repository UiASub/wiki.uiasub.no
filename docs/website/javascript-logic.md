# JavaScript & Logic

Guide to the JavaScript architecture, modules, and functionality of the UiASub website.

## JavaScript Architecture

The website uses vanilla JavaScript (ES6+) with a modular approach for maintainability.

### Script Organization

| Category | Files | Purpose |
|----------|-------|---------|
| **Core** | `header.js`, `footer.js` | Dynamic component loading |
| **Data** | `members.js`, `news-render.js` | JSON data rendering |
| **Features** | `github-projects.js`, `ui-gallery.js` | Interactive features |
| **Auth** | `login.js`, `equipment.js` | Authentication & internal tools |
| **Effects** | `carousel.js`, `waves.js`, `hero-scroll.js`, `front-page-video.js` | Visual effects |
| **PWA** | `service-worker.js`, `service-worker-register.js` | Offline functionality |
| **Utils** | `embed.js`, `specs-collapse.js` | Utility scripts |

## Core Scripts

### Header Loading (header.js)

Dynamically loads the header across all pages and handles authentication status.

**Key features**:

- Loads `header.html` into all pages
- Checks authentication status
- Updates UI based on login state
- Caches auth status for 5 minutes

**Implementation**:

```javascript
// Load header HTML
async function loadHeader() {
  const response = await fetch('/header.html')
  const html = await response.text()
  document.getElementById('header-container').innerHTML = html
  
  // Check auth status
  await updateAuthUI()
}

// Update UI based on auth
async function updateAuthUI() {
  const isLoggedIn = await checkAuthStatus()
  const loginLink = document.querySelector('#login-link')
  
  if (isLoggedIn) {
    loginLink.textContent = 'Logged In'
    loginLink.href = '/pages/equipment.html'
  } else {
    loginLink.textContent = 'Logg Inn'
    loginLink.href = '/pages/login.html'
  }
}
```

### Footer Loading (footer.js)

Similar to header, loads footer consistently across pages.

**Implementation**:

```javascript
async function loadFooter() {
  const response = await fetch('/footer.html')
  const html = await response.text()
  document.getElementById('footer-container').innerHTML = html
}

// Auto-load on page load
if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', loadFooter)
} else {
  loadFooter()
}
```

## Data Rendering

### Member Profiles (members.js)

Loads and renders team member cards from JSON.

**Features**:

- Automatic language detection
- Grouping by team section
- Responsive grid layout

**Implementation**:

```javascript
// Detect language
function detectLanguage() {
  const path = window.location.pathname
  const lang = document.documentElement.lang
  return path.includes('/en/') || lang === 'en' ? 'en' : 'no'
}

// Load members
async function loadMembers() {
  try {
    const response = await fetch('/data/members.json')
    const members = await response.json()
    const language = detectLanguage()
    
    // Group by section
    const grouped = groupBySection(members)
    
    // Render each section
    for (const [section, memberList] of Object.entries(grouped)) {
      renderSection(section, memberList, language)
    }
  } catch (error) {
    console.error('Failed to load members:', error)
    showErrorMessage()
  }
}

// Render a single member card
function renderMember(member, language) {
  const role = language === 'en' ? member.role_en : member.role_no
  
  return `
    <div class="member-card">
      <img src="/images/profie_shared/${member.image}" 
           alt="${member.name}" 
           class="member-image">
      <h3>${member.name}</h3>
      <p class="member-role">${role}</p>
    </div>
  `
}
```

### News Rendering (news-render.js)

Renders news items dynamically.

**Implementation**:

```javascript
async function loadNews() {
  try {
    const response = await fetch('/data/news.json')
    const newsItems = await response.json()
    
    const container = document.getElementById('news-container')
    container.innerHTML = newsItems
      .sort((a, b) => new Date(b.date) - new Date(a.date))
      .map(renderNewsItem)
      .join('')
  } catch (error) {
    console.error('Failed to load news:', error)
  }
}

function renderNewsItem(item) {
  return `
    <article class="news-item">
      <img src="${item.image}" alt="${item.title}">
      <div class="news-content">
        <time datetime="${item.date}">${formatDate(item.date)}</time>
        <h3>${item.title}</h3>
        <p>${item.excerpt}</p>
        <a href="${item.url}">Les mer →</a>
      </div>
    </article>
  `
}
```

## Interactive Features

### GitHub Projects (github-projects.js)

Fetches and displays GitHub repositories.

**Features**:

- GitHub API integration
- Caching to avoid rate limits
- Error handling

**Implementation**:

```javascript
const GITHUB_API = 'https://api.github.com'
const ORG_NAME = 'UiASub'
const CACHE_KEY = 'github_repos'
const CACHE_DURATION = 3600000 // 1 hour

async function fetchGitHubProjects() {
  // Check cache first
  const cached = getCachedData(CACHE_KEY)
  if (cached) return cached
  
  try {
    const response = await fetch(`${GITHUB_API}/orgs/${ORG_NAME}/repos`, {
      headers: {
        'Accept': 'application/vnd.github.v3+json'
      }
    })
    
    if (!response.ok) throw new Error('GitHub API error')
    
    const repos = await response.json()
    setCachedData(CACHE_KEY, repos)
    return repos
  } catch (error) {
    console.error('Failed to fetch GitHub projects:', error)
    return []
  }
}

function renderProject(repo) {
  return `
    <div class="project-card">
      <h3>${repo.name}</h3>
      <p>${repo.description || 'No description'}</p>
      <div class="project-meta">
        <span>⭐ ${repo.stargazers_count}</span>
        <span>🍴 ${repo.forks_count}</span>
        <span>${repo.language}</span>
      </div>
      <a href="${repo.html_url}" target="_blank">View on GitHub →</a>
    </div>
  `
}
```

### UI Gallery (ui-gallery.js)

Interactive image gallery for ROV mockups.

**Features**:

- Lightbox view
- Keyboard navigation
- Touch/swipe support

**Implementation**:

```javascript
class UIGallery {
  constructor(containerId) {
    this.container = document.getElementById(containerId)
    this.images = []
    this.currentIndex = 0
    this.init()
  }
  
  init() {
    this.loadImages()
    this.attachEventListeners()
  }
  
  loadImages() {
    const images = this.container.querySelectorAll('img')
    this.images = Array.from(images)
    
    this.images.forEach((img, index) => {
      img.addEventListener('click', () => this.openLightbox(index))
    })
  }
  
  openLightbox(index) {
    this.currentIndex = index
    const lightbox = this.createLightbox()
    document.body.appendChild(lightbox)
    document.body.style.overflow = 'hidden'
  }
  
  createLightbox() {
    const lightbox = document.createElement('div')
    lightbox.className = 'lightbox'
    lightbox.innerHTML = `
      <button class="lightbox-close">&times;</button>
      <button class="lightbox-prev">&lsaquo;</button>
      <img src="${this.images[this.currentIndex].src}" alt="Gallery image">
      <button class="lightbox-next">&rsaquo;</button>
    `
    
    // Event listeners
    lightbox.querySelector('.lightbox-close').onclick = () => this.closeLightbox()
    lightbox.querySelector('.lightbox-prev').onclick = () => this.prevImage()
    lightbox.querySelector('.lightbox-next').onclick = () => this.nextImage()
    
    return lightbox
  }
  
  nextImage() {
    this.currentIndex = (this.currentIndex + 1) % this.images.length
    this.updateLightboxImage()
  }
  
  prevImage() {
    this.currentIndex = (this.currentIndex - 1 + this.images.length) % this.images.length
    this.updateLightboxImage()
  }
}

// Initialize
new UIGallery('ui-gallery-container')
```

## Authentication

### Login (login.js)

Handles Discord OAuth flow and session management.

**Key features**:

- Discord OAuth redirect
- Token storage
- Role verification via Edge Function

**Implementation**:

```javascript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(SUPABASE_URL, SUPABASE_KEY)

async function loginWithDiscord() {
  const { error } = await supabase.auth.signInWithOAuth({
    provider: 'discord',
    options: {
      redirectTo: window.location.origin + '/pages/equipment.html'
    }
  })
  
  if (error) {
    console.error('Login error:', error)
    showError('Failed to login. Please try again.')
  }
}

async function handleCallback() {
  const { data: { session }, error } = await supabase.auth.getSession()
  
  if (error || !session) {
    window.location.href = '/pages/login.html'
    return
  }
  
  // Verify role with Edge Function
  const verified = await verifyUserRole(session.access_token)
  
  if (verified) {
    localStorage.setItem('sb-auth-token', session.access_token)
    // Redirect handled by OAuth
  } else {
    showError('You do not have permission to access this page.')
    await supabase.auth.signOut()
  }
}

async function verifyUserRole(token) {
  try {
    const response = await fetch(EDGE_FUNCTION_URL, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json'
      }
    })
    
    const data = await response.json()
    return data.verified === true
  } catch (error) {
    console.error('Role verification error:', error)
    return false
  }
}
```

### Equipment Management (equipment.js)

CRUD operations for equipment tracking.

**Implementation**:

```javascript
class EquipmentManager {
  constructor() {
    this.token = localStorage.getItem('sb-auth-token')
    if (!this.token) {
      window.location.href = '/pages/login.html'
    }
    this.init()
  }
  
  async init() {
    await this.loadEquipment()
    this.attachEventListeners()
  }
  
  async loadEquipment() {
    try {
      const response = await fetch(API_URL + '/equipment', {
        headers: { 'Authorization': `Bearer ${this.token}` }
      })
      
      const equipment = await response.json()
      this.renderEquipment(equipment)
    } catch (error) {
      console.error('Failed to load equipment:', error)
    }
  }
  
  async addEquipment(data) {
    try {
      const response = await fetch(API_URL + '/equipment', {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${this.token}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      })
      
      if (response.ok) {
        await this.loadEquipment()
        this.showSuccess('Equipment added successfully')
      }
    } catch (error) {
      console.error('Failed to add equipment:', error)
      this.showError('Failed to add equipment')
    }
  }
}

new EquipmentManager()
```

## Visual Effects

### Carousel (carousel.js)

Auto-rotating image carousel.

**Implementation**:

```javascript
class Carousel {
  constructor(containerId, options = {}) {
    this.container = document.getElementById(containerId)
    this.slides = this.container.querySelectorAll('.carousel-slide')
    this.currentIndex = 0
    this.interval = options.interval || 5000
    this.auto = options.auto !== false
    
    this.init()
  }
  
  init() {
    this.showSlide(0)
    if (this.auto) this.startAutoPlay()
    this.attachEventListeners()
  }
  
  showSlide(index) {
    this.slides.forEach(slide => slide.classList.remove('active'))
    this.slides[index].classList.add('active')
    this.currentIndex = index
  }
  
  next() {
    const nextIndex = (this.currentIndex + 1) % this.slides.length
    this.showSlide(nextIndex)
  }
  
  prev() {
    const prevIndex = (this.currentIndex - 1 + this.slides.length) % this.slides.length
    this.showSlide(prevIndex)
  }
  
  startAutoPlay() {
    this.autoPlayInterval = setInterval(() => this.next(), this.interval)
  }
  
  stopAutoPlay() {
    if (this.autoPlayInterval) {
      clearInterval(this.autoPlayInterval)
    }
  }
}

// Initialize
new Carousel('main-carousel', { interval: 6000 })
```

## Utility Functions

### Common utilities used across scripts

```javascript
// Date formatting
function formatDate(dateString, language = 'no') {
  const date = new Date(dateString)
  return date.toLocaleDateString(language === 'en' ? 'en-US' : 'nb-NO', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}

// Debounce function
function debounce(func, wait) {
  let timeout
  return function executedFunction(...args) {
    clearTimeout(timeout)
    timeout = setTimeout(() => func.apply(this, args), wait)
  }
}

// Cache management
function getCachedData(key) {
  const cached = localStorage.getItem(key)
  if (!cached) return null
  
  const { data, timestamp } = JSON.parse(cached)
  const age = Date.now() - timestamp
  
  if (age > CACHE_DURATION) {
    localStorage.removeItem(key)
    return null
  }
  
  return data
}

function setCachedData(key, data) {
  localStorage.setItem(key, JSON.stringify({
    data,
    timestamp: Date.now()
  }))
}

// Error handling
function showError(message) {
  const toast = document.createElement('div')
  toast.className = 'toast error'
  toast.textContent = message
  document.body.appendChild(toast)
  
  setTimeout(() => toast.remove(), 3000)
}
```

## Best Practices

### Error Handling

Always wrap async operations in try-catch:

```javascript
async function fetchData() {
  try {
    const response = await fetch(url)
    if (!response.ok) throw new Error(`HTTP ${response.status}`)
    return await response.json()
  } catch (error) {
    console.error('Fetch failed:', error)
    return null // or default value
  }
}
```

### Performance

1. **Debounce expensive operations**:

   ```javascript
   const handleSearch = debounce((query) => {
     performSearch(query)
   }, 300)
   ```

2. **Use event delegation**:

   ```javascript
   // Instead of attaching to each item
   container.addEventListener('click', (e) => {
     if (e.target.matches('.item')) {
       handleItemClick(e.target)
     }
   })
   ```

3. **Lazy load non-critical scripts**:

   ```javascript
   if ('IntersectionObserver' in window) {
     // Load only when needed
   }
   ```

---

[← Styling & CSS](Styling-CSS) | [PWA Features →](PWA-Features)
