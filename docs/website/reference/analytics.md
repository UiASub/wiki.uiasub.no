# Analytics

The UiASub website uses Umami Analytics for privacy-friendly website analytics and visitor insights.

## Overview

**Umami** is an open-source, privacy-focused analytics platform that provides insights into website traffic without compromising user privacy. It's GDPR-compliant and doesn't use cookies.

**Analytics Dashboard**: [https://cloud.umami.is/share/0RUudrusZxSMCnbK](https://cloud.umami.is/share/0RUudrusZxSMCnbK)

## Why Umami?

### Benefits

- **Privacy-first**: No cookies, GDPR compliant
- **Lightweight**: Minimal impact on page load times
- **Simple insights**: Clean, easy-to-understand metrics
- **No third-party data sharing**: Your data stays with you
- **Cost-effective**

### vs Google Analytics

| Feature | Umami | Google Analytics |
|---------|-------|------------------|
| **Privacy** | No cookies, GDPR compliant | Uses cookies, tracking |
| **Data ownership** | You own your data | Google owns your data |
| **Performance** | ~2KB script | ~45KB script |
| **Complexity** | Simple, clean interface | Complex, feature-heavy |
| **Cookie consent** | Not required | Required in EU |

## What We Track

### Page Views

- Total page views
- Unique visitors
- Page view trends over time
- Most visited pages

### Traffic Sources

- Referral sources (where visitors come from)
- Direct traffic
- Social media referrals
- Search engine traffic

### User Behavior

- Pages per session
- Bounce rate
- Session duration
- Entry and exit pages

### Technical Data

- Browser types
- Device types (desktop, mobile, tablet)
- Operating systems
- Screen resolutions
- Geographic locations (country level)

### Events (Custom Tracking)

- Button clicks (e.g., "Join Us" CTA)
- External link clicks (sponsors, social media)
- File downloads (e.g., PDF documents)
- Form submissions (contact form)

## What We DON'T Track

- Personal information
- IP addresses (anonymized)
- Cookies or local storage
- Individual user journeys
- Sensitive user data

## Implementation

### Analytics Script

The Umami tracking script is included in the site's HTML:

```html
<script
  async
  src="https://cloud.umami.is/script.js"
  data-website-id="your-website-id"
></script>
```

**Location**: Add to the `<head>` section of all HTML pages.

### Custom Event Tracking

Track specific user interactions using the `umami` object:

```javascript
// Track button clicks
document.getElementById('join-button').addEventListener('click', () => {
  umami.track('join-button-click')
})

// Track outbound links
document.querySelectorAll('a[href^="http"]').forEach(link => {
  link.addEventListener('click', (e) => {
    const url = e.target.href
    umami.track('outbound-link', { url })
  })
})

// Track form submissions
document.getElementById('contact-form').addEventListener('submit', () => {
  umami.track('contact-form-submit')
})

// Track file downloads
document.querySelectorAll('a[download]').forEach(link => {
  link.addEventListener('click', (e) => {
    const file = e.target.getAttribute('download')
    umami.track('file-download', { file })
  })
})
```

### Page View Tracking

Page views are automatically tracked by the Umami script. No additional code needed.

### Event Properties

Add custom properties to events for more detailed insights:

```javascript
// Track with custom properties
umami.track('sponsor-click', {
  sponsor: 'Company Name',
  tier: 'main'
})

umami.track('language-switch', {
  from: 'no',
  to: 'en'
})

umami.track('rov-3d-model-view', {
  duration: sessionDuration
})
```

## Accessing Analytics

### Public Dashboard

Anyone can view the public analytics dashboard:

**Link**: [https://cloud.umami.is/share/0RUudrusZxSMCnbK](https://cloud.umami.is/share/0RUudrusZxSMCnbK)

**Available metrics**:

- Total visitors and page views
- Top pages
- Referral sources
- Geographic distribution
- Device and browser breakdown

### Admin Dashboard

For full access and detailed analytics:

1. Log in to [cloud.umami.is](https://cloud.umami.is)
2. Navigate to the UiASub website dashboard
3. View detailed reports and real-time data

**Admin access**: Contact the team leadership for credentials.

## Key Metrics to Monitor

### Traffic Growth

- **Total visitors**: Track month-over-month growth
- **Unique visitors**: Measure reach
- **Page views per visitor**: Engagement indicator

### Popular Content

- **Most visited pages**: Which content resonates
- **High bounce rate pages**: Pages that need improvement
- **Entry pages**: Where users typically land

### Recruitment Success

- **Join page visits**: Track interest in joining
- **Join button clicks**: Measure conversion intent
- **Form submissions**: Actual conversions

### Sponsor Engagement

- **Sponsor page visits**: Sponsor visibility
- **Sponsor link clicks**: Engagement with partners
- **Click-through rate**: Effectiveness of sponsor placement

### Technical Insights

- **Mobile vs desktop**: Optimize for primary device type
- **Browser distribution**: Ensure compatibility
- **Load times**: Performance monitoring

## Setting Up Custom Events

### Example: Track Member Profile Views

```javascript
// In members.js
function renderMember(member, language) {
  const card = document.createElement('div')
  card.className = 'member-card'
  // ... render card content
  
  card.addEventListener('click', () => {
    umami.track('member-profile-view', {
      member: member.name,
      role: language === 'en' ? member.role_en : member.role_no,
      group: member.group
    })
  })
  
  return card
}
```

### Example: Track News Article Reads

```javascript
// In news-render.js
function renderNewsItem(item) {
  const article = document.createElement('article')
  // ... render article
  
  article.querySelector('.read-more').addEventListener('click', () => {
    umami.track('news-article-read', {
      title: item.title_no,
      category: item.category,
      date: item.date
    })
  })
  
  return article
}
```

### Example: Track ROV 3D Model Interactions

```javascript
// In rov.js or ui-gallery.js
const modelViewer = document.querySelector('.rov-viewer')

modelViewer.addEventListener('click', () => {
  umami.track('rov-model-interaction', {
    action: 'view'
  })
})

// Track rotation
let rotationStarted = false
modelViewer.addEventListener('mousemove', () => {
  if (!rotationStarted) {
    umami.track('rov-model-interaction', {
      action: 'rotate'
    })
    rotationStarted = true
  }
})
```

## Privacy Compliance

### GDPR Compliance

Umami is GDPR-compliant by design:

- No cookies used
- No personal data collected
- Anonymized IP addresses
- No cross-site tracking
- User data not sold or shared

### No Cookie Banner Required

Because Umami doesn't use cookies, you don't need a cookie consent banner for analytics purposes.

### Data Retention

- **Default**: 365 days
- **Configurable**: Can be adjusted in settings
- **Deletion**: Data can be manually deleted anytime

## Troubleshooting

### Analytics not tracking

**Check**:

1. Script is loaded (check browser DevTools → Network tab)
2. Correct website ID in script tag
3. No ad blockers preventing script
4. Script not blocked by CSP (Content Security Policy)

**Debug**:

```javascript
// Check if umami is loaded
if (typeof umami !== 'undefined') {
  console.log('Umami loaded successfully')
} else {
  console.error('Umami not loaded')
}

// Test tracking manually
umami.track('test-event')
```

### Events not showing up

**Common issues**:

- Event name typos
- Script not loaded before event tracking code
- Ad blockers blocking requests
- Events filtered out in dashboard

**Solution**:

```javascript
// Wait for umami to load
window.addEventListener('load', () => {
  if (typeof umami !== 'undefined') {
    // Track events here
    umami.track('page-ready')
  }
})
```

### Dashboard not updating

- Data updates every few minutes (not real-time)
- Clear browser cache if dashboard looks outdated
- Check date range filter in dashboard

## Best Practices

### Event Naming

Use consistent, descriptive names:

```javascript
// Good: Clear, consistent
umami.track('join-button-click')
umami.track('sponsor-logo-click')
umami.track('contact-form-submit')

// Avoid: Vague, inconsistent
umami.track('click')
umami.track('Button Clicked')
umami.track('user_action')
```

### Don't Over-Track

Track meaningful actions, not everything:

```javascript
// Track important actions
umami.track('recruitment-application-started')
umami.track('rov-technical-docs-downloaded')

// Don't track trivial actions
umami.track('mouse-moved')
umami.track('scrolled-5px')
```

### Use Event Properties

Add context to events:

```javascript
// Good: Includes useful context
umami.track('news-filter-applied', {
  category: 'achievement',
  language: 'no'
})

// Okay but less useful
umami.track('news-filter-applied')
```

### Performance

Keep tracking lightweight:

```javascript
// Good: Debounced tracking
const trackScroll = debounce(() => {
  umami.track('article-read-50-percent')
}, 1000)

window.addEventListener('scroll', trackScroll)
```

## Reporting

### Monthly Reports

Use analytics to create monthly reports tracking:

1. **Traffic metrics**: Visitors, page views, growth
2. **Top content**: Most popular pages
3. **Referral sources**: Where traffic comes from
4. **User engagement**: Bounce rate, session duration
5. **Goals**: Recruitment clicks, form submissions

### Sharing Insights

The public dashboard link can be shared with:

- Team members
- Sponsors (show reach and engagement)
- University administration
- Recruitment materials

## Future Enhancements

### Goals & Funnels

Set up conversion funnels:

1. Homepage visit → About page → Join page → Form submit
2. Landing page → Sponsor page → Sponsor link click

### A/B Testing

Track different versions:

```javascript
const variant = Math.random() < 0.5 ? 'A' : 'B'

umami.track('cta-button-click', {
  variant: variant
})
```

### Real-Time Monitoring

Monitor live visitors during:

- Competition events
- Recruitment drives
- Launch announcements

---

[← PWA Features](PWA-Features) | [Back to Home](Home)
