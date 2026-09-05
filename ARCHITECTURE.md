# Architecture Overview
## Ismael Asumanu Prince — Graphic Designer Portfolio

**Document Version:** 1.0  
**Last Updated:** September 2026  
**Purpose:** Technical architecture and system design documentation

---

## System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                     GitHub Pages Hosting                         │
│              (https://mawuliprince.github.io/...)               │
└──────────────────────────┬──────────────────────────────────────┘
                           │
           ┌───────────────┼───────────────┐
           │               │               │
      ┌────▼────┐   ┌─────▼──────┐   ┌───▼──────────┐
      │ index   │   │  about     │   │  projects    │
      │ .html   │   │  .html     │   │  .html       │
      └────┬────┘   └─────┬──────┘   └───┬──────────┘
           │               │               │
           └───────────────┼───────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
   ┌────▼─────┐       ┌───▼─────┐      ┌────▼────────┐
   │ Vanilla  │       │ Vanilla │      │ Vanilla     │
   │ HTML5    │       │ CSS3    │      │ JavaScript  │
   │ Markup   │       │ Styling │      │ (ES6+)      │
   └────┬─────┘       └───┬─────┘      └────┬────────┘
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
   ┌────▼──────────┐  ┌───▼────────┐  ┌─────▼──────────┐
   │ External APIs │  │  CMS Data  │  │  Media & CDN   │
   │ & Services    │  │  (JSON)    │  │  (Cloudinary)  │
   └────┬──────────┘  └───┬────────┘  └─────┬──────────┘
        │                  │                  │
   ┌────┴────────┬─────────┴─────┬───────────┴────┐
   │             │               │                 │
   ▼             ▼               ▼                 ▼
Google       GitHub API    site-home.json    Cloudinary
Analytics   raw.githubusercontent.com    image/video
            (cache busting)               optimization
   
Microsoft
Clarity
```

---

## Application Layers

### 1. **Presentation Layer (Frontend)**

#### HTML Structure
- **Single Page Architecture** with multiple HTML files serving as "pages"
- **Semantic HTML5:** Proper use of `<header>`, `<nav>`, `<section>`, `<footer>`
- **Accessibility:** ARIA labels, semantic elements for screen readers
- **Meta tags:** SEO metadata, Open Graph for social sharing

**Core HTML Pages:**
```
index.html      → Home/Landing page (hero + footer)
about.html      → Designer bio, education, skills
projects.html   → Portfolio gallery
studio.html     → Admin dashboard
booking.html    → Consultation booking
admin (1).html  → Admin interface
404.html        → Error page
```

#### CSS Architecture
- **CSS Custom Properties (Variables):** Theme colors, sizing units
- **Mobile-first responsive design:** `@media` queries for breakpoints
- **CSS Grid & Flexbox:** Modern layout techniques
- **Smooth transitions & animations:** Cubic-bezier easing, keyframe animations
- **CSS BEM-lite naming:** Descriptive class names

**Breakpoints:**
```css
Mobile:  < 600px
Tablet:  600px - 1024px
Desktop: > 1024px
```

**Theme Variables:**
```css
:root {
  --bg, --surface, --text, --muted
  --border, --accent, --accent-dim
  --footer-bg, --footer-text
}

body.dark { /* Override with dark theme */ }
```

---

### 2. **Data Layer (CMS)**

#### JSON-Based Content Management
The portfolio uses static JSON files as a lightweight CMS:

**site-home.json**
```json
{
  "name": "Prince Ismeal Asumanu",
  "role": "Graphic Designer · Brand Identity Designer",
  "heroBg": "https://res.cloudinary.com/.../video.mp4",
  "contactDesc": "",
  "email": "princeismaelasumanu@gmail.com",
  "linkedin": "...",
  "dribbble": "...",
  "instagram": "",
  "behance": ""
}
```

**site-about.json**
```json
{
  "aboutShort": "...",
  "aboutFull": "...",
  "eduShort": "...",
  "eduFull": "<h3>...",
  "workShort": "...",
  "workFull": "...",
  "skillsShort": "...",
  "skillsFull": "...",
  "reviews": [],
  "expStart": "2023",
  "projectsCount": "500+"
}
```

**projects.json**
```json
[
  {
    "id": "proj_1781062806683",
    "title": "MIKITA",
    "subtitle": "Fine Jewelry Brand Identity & Concept",
    "description": "<div>...",
    "tags": ["Branding", "Logo Design", ...],
    "year": "2026",
    "media": [
      { "url": "...", "type": "image|video" }
    ],
    "isFeatured": false,
    "isHidden": false,
    "projectOrder": 0
  }
]
```

#### Data Fetching Strategy
```javascript
// Cache-busting fetch from GitHub raw CDN
const SITE_URL = 'https://raw.githubusercontent.com/MawuliPRINCE/my-portfolio/main/site-home.json';
fetch(SITE_URL + '?t=' + Date.now())
  .then(r => r.ok ? r.json() : null)
  .then(d => { /* Apply data to DOM */ })
  .catch(() => { /* Fallback to hardcoded defaults */ });
```

**Benefits:**
- No backend required (purely static)
- Version controlled in Git
- Cache-busting via timestamp parameter
- Graceful degradation with fallback values

---

### 3. **Business Logic Layer (JavaScript)**

#### Core JavaScript Modules

**1. Theme Management**
```javascript
function toggleTheme() {
  document.body.classList.toggle('dark');
  localStorage.setItem('theme', isDark ? 'dark' : 'light');
}

// Detect system preference on load
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
```

**2. Navigation & Menu**
```javascript
function openMenu() { 
  menuEl.classList.add('open'); 
  backdrop.classList.add('open'); 
  document.body.style.overflow = 'hidden'; 
}

function closeMenu() { 
  menuEl.classList.remove('open'); 
  backdrop.classList.remove('open'); 
  document.body.style.overflow = ''; 
}

// Touch gesture support for mobile
menuEl.addEventListener('touchend', e => {
  if (swipeDelta > 60) closeMenu();
});

// Keyboard support
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') closeMenu();
});
```

**3. Header Scroll State**
```javascript
function handleScroll() {
  if (window.scrollY > 60) {
    hdr.classList.remove('at-top');
    hdr.classList.add('scrolled');
  } else {
    hdr.classList.add('at-top');
    hdr.classList.remove('scrolled');
  }
}
```

**4. Scroll Progress Tracking**
```javascript
function updateScrollProgress() {
  const scrollPercent = scrollY / (docHeight - viewportHeight);
  progressCircle.style.strokeDashoffset = 
    circumference - (scrollPercent * circumference);
  
  if (scrollY > 300) {
    scrollBtn.classList.add('visible');
  }
}
```

**5. Internationalization (i18n)**
```javascript
const i18n = {
  en: { 'nav-home': 'Home', 'hero-role': 'Graphic Designer...', ... },
  fr: { 'nav-home': 'Accueil', 'hero-role': 'Designer Graphique...', ... },
  es: { /* ... */ },
  de: { /* ... */ }
};

function applyLang(lang) {
  document.querySelectorAll('[data-i18n]').forEach(el => {
    const key = el.getAttribute('data-i18n');
    el.innerHTML = i18n[lang][key];
  });
}
```

**6. Page Transitions**
```javascript
document.querySelectorAll('a').forEach(link => {
  if (link.hostname === window.location.hostname) {
    link.addEventListener('click', e => {
      e.preventDefault();
      document.body.classList.add('fade-out');
      setTimeout(() => { window.location = href; }, 300);
    });
  }
});
```

**7. Scroll Reveal Animations**
```javascript
const scrollRevealObserver = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('revealed');
      scrollRevealObserver.unobserve(entry.target);
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('.scroll-reveal').forEach(el => {
  scrollRevealObserver.observe(el);
});
```

**8. Dynamic Content Injection**
```javascript
// Inject hero background (image or video)
if (d.heroBg) {
  const isVid = /\.(mp4|webm|mov|ogg)/i.test(d.heroBg);
  const media = isVid ? 
    createVideoElement(d.heroBg) : 
    createImageElement(d.heroBg);
  hero.insertBefore(media, hero.firstChild);
}

// Inject social links from CMS data
const socials = {
  'social-email': 'mailto:' + d.email,
  'social-linkedin': d.linkedin,
  'social-dribbble': d.dribbble
};
```

---

### 4. **External Integrations Layer**

#### Analytics & Tracking
```
Google Analytics 4
├── Tracking ID: G-RNQ0C2YB4D
├── Events tracked: pageview, button clicks
├── Goals: contact clicks, project views
└── Audience: potential clients, design enthusiasts

Microsoft Clarity
├── Project ID: xbuxkrh18o
├── Session recording
├── Heatmap analysis
└── User behavior insights
```

#### Content Delivery Network (Cloudinary)
```
Primary use: Image & video optimization
├── Auto-compression: ?q_auto,f_auto
├── Responsive images: Multiple sizes
├── Video streaming: Adaptive bitrate
└── CDN caching: Global edge servers
```

#### GitHub API Integration
```javascript
// Fetch JSON from GitHub raw CDN (cache-busting)
const endpoint = 'https://raw.githubusercontent.com/MawuliPRINCE/my-portfolio/main/site-home.json';
const response = await fetch(endpoint + '?t=' + Date.now());
```

---

## Request/Response Flows

### Flow 1: Initial Page Load (index.html)
```
1. Browser requests: https://mawuliprince.github.io/my-portfolio/
2. GitHub Pages serves: index.html
3. HTML parsed, CSS loaded, inline styles applied
4. JavaScript executes:
   - Detect theme preference
   - Set year in footer
   - Bind event listeners
5. Fetch site-home.json from GitHub raw CDN
6. Inject hero background & social links
7. Fetch projects.json for lazy-loading galleries
8. Google Analytics & Clarity scripts initialize
9. Page interactive (complete)
```

### Flow 2: Theme Toggle
```
1. User clicks lightbulb icon in header
2. JavaScript toggles 'dark' class on <body>
3. CSS variables automatically update via cascade
4. localStorage.setItem('theme', 'dark') saves preference
5. All elements re-render with dark color scheme
6. Preference persists on next visit
```

### Flow 3: Language Switch
```
1. User selects language from dropdown (e.g., "FR")
2. JavaScript calls switchLang('fr')
3. applyLang('fr') updates all [data-i18n] elements
4. localStorage.setItem('lang', 'fr') saves preference
5. document.documentElement.lang = 'fr' updates HTML lang attribute
6. Browser/assistive tech recognizes French content
```

### Flow 4: Navigation (with Page Transition)
```
1. User clicks navigation link (e.g., "Projects")
2. JavaScript intercepts click, prevents default
3. Body element fades out (fade-out class)
4. After 300ms, window.location = 'projects.html'
5. Browser navigates to new URL
6. New HTML loads, CSS resets, JavaScript reinitializes
7. Fade-in animation (reverse of fade-out)
8. User sees smooth transition between pages
```

---

## Data Flow Diagram

```
┌──────────────────────────────────────────────────────────┐
│                   User Interaction                       │
│        (Click, Scroll, Type, Touch, Keyboard)           │
└──────────────────┬───────────────────────────────────────┘
                   │
        ┌──────────▼──────────┐
        │  Event Listener     │
        │  (JS handlers)      │
        └──────────┬──────────┘
                   │
        ┌──────────▼──────────┐
        │  State Update       │
        │  (localStorage,     │
        │   classList, etc.)  │
        └──────────┬──────────┘
                   │
        ┌──────────▼──────────┐
        │  DOM Manipulation   │
        │  (classList.add,    │
        │   innerHTML, etc.)  │
        └──────────┬──────────┘
                   │
        ┌──────────▼──────────┐
        │  CSS Rendering      │
        │  (Repaint/Reflow)   │
        └──────────┬──────────┘
                   │
        ┌──────────▼──────────┐
        │  Visual Update      │
        │  (User sees change) │
        └─────────────────────┘
```

---

## Performance Optimization Strategies

### 1. **Image Optimization**
- Cloudinary auto-compression: `?q_auto,f_auto`
- Responsive images with device pixel ratio detection
- Lazy loading for below-fold images (IntersectionObserver)
- WebP format support (Cloudinary automatic)

### 2. **Caching Strategy**
```
Static assets (HTML, CSS, JS):
├── GitHub Pages handles (ETags, Cache-Control headers)
└── Browser caches for ~1 hour

JSON data (CMS content):
├── Cache-busting timestamp: ?t=Date.now()
├── Prevents stale data issues
└── API caches for ~5 minutes on CDN

Media (Cloudinary):
├── Long-term cache (1 year)
├── Immutable URLs with version hashes
└── Global CDN edge caching
```

### 3. **Code Splitting (Implicit)**
```
Each HTML page is separate:
├── index.html - minimal JavaScript (hero, navigation)
├── about.html - about-specific code
├── projects.html - gallery/filtering logic
└── User only loads JS for visited pages
```

### 4. **Lighthouse Optimization**
- **Performance:** Lazy load images, minimize CLS, optimize FCP
- **Accessibility:** Semantic HTML, ARIA labels, color contrast
- **Best Practices:** HTTPS only, CSP headers, no console errors
- **SEO:** Meta tags, structured data, mobile-friendly

---

## Security Architecture

### Content Security Policy (CSP) Header
```
default-src 'self';
script-src 'self' 'unsafe-inline' 
  https://www.googletagmanager.com 
  https://cdn.jsdelivr.net 
  https://www.clarity.ms;
style-src 'self' 'unsafe-inline' 
  https://fonts.googleapis.com 
  https://cdnjs.cloudflare.com;
font-src 'self' 
  https://fonts.gstatic.com 
  https://cdnjs.cloudflare.com;
img-src 'self' data: https://*.cloudinary.com;
connect-src 'self' 
  https://raw.githubusercontent.com 
  https://api.github.com 
  https://www.google-analytics.com;
frame-src 'self' https://analytics.google.com;
```

**Security Benefits:**
- Prevents XSS (cross-site scripting) attacks
- Restricts resource loading to trusted origins
- Mitigates clickjacking via frame-src
- Only allows HTTPS connections

### HTTPS & Domain Security
- GitHub Pages enforces HTTPS
- Certificate auto-renewed by GitHub
- HSTS headers enabled (subdomains secure)

---

## Scalability Considerations

### Current Architecture (Fits Current Needs)
✅ Static site generator approach
✅ JSON-based CMS (no database)
✅ Client-side rendering only
✅ ~100KB total payload (without images)

### Future Scalability (Phase 2+)
- **Blog/CMS:** Consider headless CMS (Contentful, Strapi)
- **Backend API:** Serverless functions (Vercel, Cloudflare Workers) for form submissions
- **Database:** Firebase Realtime DB for bookings/inquiries
- **Build system:** Static site generator (11ty, Hugo) for template reuse

---

## Deployment Architecture

### GitHub Pages Workflow
```
Push to main branch
        │
        ▼
GitHub Actions CI/CD (optional)
        │
        ├─ Lint HTML/CSS/JS
        ├─ Performance checks
        └─ Deploy to Pages
        │
        ▼
GitHub Pages Build
        │
        ├─ Build Jekyll (if configured)
        └─ Serve static files
        │
        ▼
GitHub CDN (Global Edge Servers)
        │
        ▼
User's Browser
```

### DNS & Domain Setup
```
Domain: mawuliprince.github.io (subdomain)
Protocol: HTTPS (GitHub auto-managed)
CDN: GitHub's global edge network
Certificate: Let's Encrypt (auto-renewed)
TTL: Varies by GitHub CDN
```

---

## Error Handling & Fallbacks

### JSON Fetch Failures
```javascript
fetch(jsonUrl)
  .then(r => r.ok ? r.json() : null)
  .then(d => {
    if (!d) return; // Silently fail
    // Apply data to DOM
  })
  .catch(() => {
    // Fallback: use hardcoded defaults
    document.getElementById('hero-role').textContent = 
      'Graphic Designer · Brand Identity Designer';
  });
```

### Missing Assets
- Image not found → Cloudinary serves placeholder
- Video not available → Hide video element
- JSON unavailable → Use hardcoded fallback content

### Browser Compatibility
- CSS fallbacks for Grid/Flexbox
- JavaScript feature detection
- Graceful degradation for animations

---

## Monitoring & Diagnostics

### Google Analytics 4 Tracking
```
Events tracked:
├── page_view (all pages)
├── scroll (engagement metric)
├── click (social links, CTAs)
└── engagement_time (session duration)

Custom reports:
├── Traffic source analysis
├── Project page popularity
├── Contact conversion funnel
└── Device/browser breakdown
```

### Microsoft Clarity
```
Recordings: User session heatmaps
Analysis:
├── Where users click
├── Scroll depth
├── Session recordings (anonymized)
└── Rage clicks/errors
```

### Error Tracking (Future)
- Sentry integration (optional)
- Console error logging
- JavaScript error reporting

---

## Maintenance & Ops

### Version Control
```
Git repository: MawuliPRINCE/my-portfolio
Main branch: Production deployment
Workflow:
  1. Make changes locally
  2. Commit with descriptive message
  3. Push to main
  4. GitHub Pages auto-deploys in ~1 minute
```

### Content Updates
```
To update project:
  1. Edit projects.json
  2. Commit: "Update: Add new project"
  3. Push to main
  4. Site refreshes in ~1 minute
  
To update bio:
  1. Edit site-about.json
  2. Commit: "Update: Bio revision"
  3. Push to main
  4. About page reflects change on next visit
```

### Performance Monitoring
```
Monthly reviews:
├── Lighthouse scores (PageSpeed Insights)
├── Google Analytics reports
├── Clarity heatmaps & recordings
├── Core Web Vitals (CLS, FID, LCP)
└── 404 error logs
```

---

## Technology Decisions & Rationale

| Technology | Why Used | Alternatives Considered |
|---|---|---|
| Vanilla HTML/CSS/JS | No build step needed, maximum compatibility | React, Vue, Svelte |
| JSON CMS | Simple, version-controlled, no backend | Contentful, Strapi, Prismic |
| Cloudinary | Image optimization, global CDN | AWS S3, Imgix, Netlify |
| GitHub Pages | Free, integrated with Git, auto-deploys | Vercel, Netlify, custom server |
| Google Analytics 4 | Industry standard, free tier sufficient | Plausible, Fathom, Mixpanel |

---

## Future Architecture Evolution

### Phase 2: Enhanced Backend
```
Current: Static HTML + JSON CMS
          │
          ▼
Phase 2:  Static HTML + Headless CMS + Serverless Functions
          │
          ├─ Booking system (Stripe integration)
          ├─ Contact form submissions (email)
          ├─ Blog with dynamic content
          └─ Admin dashboard for content updates
```

### Phase 3: Full-Stack Transformation
```
Phase 3:  Next.js/Remix Frontend + Backend API + Database
          │
          ├─ Real-time project updates
          ├─ Client portal
          ├─ E-commerce (digital products)
          ├─ Advanced analytics
          └─ Content management UI
```

---

## Architecture Checklist

- ✅ Single Page Application (HTML-based)
- ✅ Responsive mobile-first design
- ✅ Dark/Light theme support
- ✅ Internationalization (i18n)
- ✅ Analytics & tracking
- ✅ SEO optimization
- ✅ Performance optimization
- ✅ Security (CSP headers)
- ✅ Accessibility (semantic HTML, ARIA)
- ✅ Error handling & fallbacks
- ✅ Version control & deployment automation
- ✅ CMS-driven content (JSON-based)

---

**End of Architecture Document**

*This document should be reviewed and updated as the portfolio evolves and new technologies are integrated.*
