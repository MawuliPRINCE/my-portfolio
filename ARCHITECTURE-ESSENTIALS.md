# Architecture Essentials
## Quick Reference Guide for Ismael's Portfolio

**Purpose:** A condensed, developer-focused guide for quick lookups and onboarding.

---

## 30-Second Overview

**What:** Personal portfolio website for a graphic designer  
**Where:** GitHub Pages (mawuliprince.github.io/my-portfolio)  
**How:** Vanilla HTML/CSS/JS + JSON CMS + Cloudinary  
**Why:** Simple, fast, no backend needed, Git-versioned content

---

## Tech Stack at a Glance

```
Frontend:     HTML5 + CSS3 + JavaScript (ES6+)
Hosting:      GitHub Pages
CMS:          JSON files (site-home.json, site-about.json, projects.json)
Media:        Cloudinary CDN
Analytics:    Google Analytics 4 + Microsoft Clarity
APIs:         GitHub raw CDN, Google Analytics
```

---

## File Structure (Essentials)

```
my-portfolio/
├── index.html           Home page (hero + footer)
├── about.html           Bio, education, skills
├── projects.html        Portfolio gallery
├── 404.html             Error page
├── site-home.json       Home CMS data (hero, socials)
├── site-about.json      About CMS data (bio, skills)
├── projects.json        Projects array (6 projects)
├── ARCHITECTURE.md      Full tech docs
└── PRD.md               Product requirements
```

---

## Key Directories (What Does What)

| File | Purpose | Edit When |
|---|---|---|
| `index.html` | Landing page template | Changing hero layout, footer design |
| `about.html` | About page template | Updating education, skills section |
| `projects.html` | Gallery template | Adding filtering, changing layout |
| `site-home.json` | Hero content | Changing name, email, social links |
| `site-about.json` | Bio content | Updating bio, education, experience |
| `projects.json` | Portfolio projects | Adding/removing projects |

---

## How It Works (The Flow)

### 1. User Visits Site
```
1. Browser → GitHub Pages → index.html
2. HTML renders, CSS loads, JS initializes
3. JavaScript detects theme preference (dark/light)
4. JavaScript fetches site-home.json from GitHub CDN
5. Content injected into DOM dynamically
6. Analytics scripts load (Google & Clarity)
```

### 2. User Interacts
```
Click Theme Toggle:
  → JS toggles 'dark' class on <body>
  → CSS variables change colors instantly
  → Preference saved to localStorage

Click Language Dropdown:
  → JS calls applyLang('fr')
  → All [data-i18n] elements update
  → Language saved to localStorage

Click Navigation Link:
  → JS intercepts, fades out page
  → Navigates to new HTML file
  → New page fades in with smooth transition
```

### 3. Content Updates
```
Update hero background:
  1. Edit site-home.json → change "heroBg" value
  2. Commit & push
  3. Site updates on next visitor (cache-busted with ?t=Date.now())

Add new project:
  1. Add project object to projects.json
  2. Commit & push
  3. Gallery updates automatically

Update bio:
  1. Edit site-about.json → change "aboutFull" value
  2. Commit & push
  3. About page reflects change on next visit
```

---

## CSS Architecture (Quick Guide)

### Theme Colors (CSS Variables)
```css
:root {
  --bg: #faf9f7;              /* Page background */
  --surface: #f2f0ec;         /* Card/surface background */
  --text: #111;               /* Body text */
  --muted: #888;              /* Secondary text */
  --border: #e0ddd8;          /* Dividers */
  --accent: #ff4500;          /* Orange-red highlight */
  --footer-bg: #0d0d0d;       /* Footer background */
  --footer-text: #f0ede8;     /* Footer text */
}

body.dark {
  --bg: #0d0d0d;
  --text: #f0ede8;
  /* Other variables inverted */
}
```

### Key Classes (BEM-lite)
```css
.hero              Hero section
.hero-bg           Background image/video
.hero-content      Text content overlay
.header.scrolled   Header state when scrolling
.menu.open         Mobile menu expanded
.scroll-reveal     Animation class for scroll-in
.fade-out          Page transition effect
```

### Responsive Breakpoints
```css
@media (max-width: 600px)      Mobile styles
/* No media query = Desktop first fallback */
```

---

## JavaScript Modules (Quick Reference)

| Module | What It Does | Key Functions |
|---|---|---|
| **Theme** | Dark/light mode toggling | `toggleTheme()` |
| **Menu** | Mobile navigation | `openMenu()`, `closeMenu()` |
| **Navigation** | Page transitions with fade | Intercepts link clicks |
| **Header Scroll** | Sticky header state | `handleScroll()` |
| **Scroll Progress** | Progress ring indicator | `updateScrollProgress()` |
| **i18n** | Language switching | `switchLang()`, `applyLang()` |
| **CMS Data** | Fetch & inject content | `fetch()` + `innerHTML` |
| **Scroll Reveal** | Animations on scroll | IntersectionObserver |

---

## CMS Data Structure (JSON Schema)

### site-home.json
```json
{
  "name": "string",           // Hero name
  "role": "string",           // Hero role/title
  "heroBg": "url",            // Hero background image/video
  "contactDesc": "string",    // Contact section description
  "email": "string",          // Email address
  "linkedin": "url",          // LinkedIn profile
  "dribbble": "url",          // Dribbble profile
  "instagram": "url",         // Instagram profile
  "behance": "url"            // Behance profile
}
```

### site-about.json
```json
{
  "aboutShort": "string",     // Short bio
  "aboutFull": "string|html", // Full bio (can be HTML)
  "eduShort": "string",       // Short education
  "eduFull": "string|html",   // Full education with HTML
  "workShort": "string",      // Short experience
  "workFull": "string|html",  // Full experience with HTML
  "skillsShort": "string",    // Short skills list
  "skillsFull": "string|html", // Full skills with HTML
  "reviews": "array",         // Testimonials array
  "expStart": "string",       // Start year (e.g., "2023")
  "projectsCount": "string"   // Total projects (e.g., "500+")
}
```

### projects.json
```json
[
  {
    "id": "string",                    // Unique ID
    "title": "string",                 // Project title
    "subtitle": "string",              // Project subtitle/category
    "description": "string|html",      // Detailed description
    "tags": ["string"],                // Categories/tags
    "year": "string",                  // Year completed
    "media": [                         // Images & videos
      { "url": "string", "type": "image|video" }
    ],
    "isFeatured": boolean,             // Highlight in gallery
    "isHidden": boolean,               // Hide from public
    "projectOrder": number,            // Sort order
    "collaborators": ["string"],       // Team members
    "seoTitle": "string",              // SEO metadata
    "seoDescription": "string",
    "seoImage": "url",
    "clientTestimonial": "string"      // Client quote
  }
]
```

---

## Data Fetching (How It Works)

```javascript
// Step 1: Construct URL with cache-busting timestamp
const SITE_URL = 'https://raw.githubusercontent.com/MawuliPRINCE/my-portfolio/main/site-home.json';
const url = SITE_URL + '?t=' + Date.now();

// Step 2: Fetch from GitHub CDN
fetch(url)
  .then(response => {
    if (response.ok) return response.json();
    return null; // Fail gracefully
  })
  .then(data => {
    if (!data) return; // Use hardcoded defaults
    
    // Step 3: Inject data into DOM
    document.getElementById('hero-role').textContent = data.role;
    document.getElementById('social-email').href = 'mailto:' + data.email;
    // ... etc
  })
  .catch(error => {
    // Fallback to hardcoded values
    console.error('Failed to fetch CMS data', error);
  });
```

**Why cache-busting?** Ensures visitors always see latest content (no browser cache issues)

---

## Deployment Process

### Code Flow
```
1. Edit files locally
2. Git add → git commit → git push
3. GitHub detects push to main branch
4. GitHub Pages rebuilds (usually <1 min)
5. Site updates at https://mawuliprince.github.io/my-portfolio/
```

### Content Updates (No Code Needed)
```
1. Open site-home.json / projects.json in GitHub UI
2. Click Edit (pencil icon)
3. Make changes
4. Commit with message
5. Auto-deploys in <1 minute
6. Visitors see changes on next visit (cache-busted)
```

---

## Performance Tips

### Image Optimization
```
✅ Always use Cloudinary URLs
✅ Cloudinary auto-optimizes with ?q_auto,f_auto
✅ Responsive images loaded at appropriate sizes
✅ Use WebP format when available

❌ Don't: Upload large unoptimized images
```

### Caching Strategy
```
GitHub Pages files:    Cached by browser (~1 hour)
JSON CMS data:        Cache-busted with ?t=Date.now()
Cloudinary images:    Long-term cache (1 year)
```

### Rendering Performance
```
✅ Vanilla JS (no framework overhead)
✅ CSS Grid/Flexbox (efficient layout)
✅ IntersectionObserver (efficient scroll detection)
✅ Event delegation (fewer listeners)

❌ Avoid: Heavy animations, large bundles, synchronous operations
```

---

## Analytics Integration

### Google Analytics 4
```javascript
// Tracking ID: G-RNQ0C2YB4D
// Auto-tracks: pageviews, scroll depth, engagement
// Custom events can be added with gtag()
```

### Microsoft Clarity
```javascript
// Project ID: xbuxkrh18o
// Records: User sessions, heatmaps, interaction patterns
// 1000s sessions/month free tier
```

### Viewing Reports
- **Google Analytics:** https://analytics.google.com
- **Clarity:** https://clarity.microsoft.com

---

## Security Essentials

### Content Security Policy (CSP)
```html
<!-- Restricts what resources can load -->
<meta http-equiv="Content-Security-Policy" 
  content="
    default-src 'self';
    script-src 'self' 'unsafe-inline' https://www.googletagmanager.com;
    style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
    img-src 'self' data: https://*.cloudinary.com;
  ">
```

### HTTPS Enforcement
```
✅ GitHub Pages enforces HTTPS by default
✅ Auto-renewed SSL certificates
✅ All traffic encrypted (TLS 1.3)
```

### Best Practices
```
✅ Validate all user input (future forms)
✅ Sanitize HTML content from CMS
✅ Never store sensitive data in localStorage
✅ Use HTTPS for all external APIs
```

---

## Common Tasks & How-Tos

### Change Hero Background
**File:** `site-home.json`
```json
{
  "heroBg": "https://res.cloudinary.com/dtjahgffs/video/upload/v1781897914/NEW_VIDEO.mp4"
}
```

### Update Social Links
**File:** `site-home.json`
```json
{
  "email": "newemail@example.com",
  "linkedin": "https://linkedin.com/in/newprofile",
  "dribbble": "https://dribbble.com/newhandle"
}
```

### Add New Project
**File:** `projects.json` - Add object to array:
```json
{
  "id": "proj_" + Date.now(),
  "title": "Project Title",
  "subtitle": "Category",
  "description": "<div>Case study content...</div>",
  "tags": ["Tag1", "Tag2"],
  "year": "2026",
  "media": [
    { "url": "https://cloudinary.com/.../image.jpg", "type": "image" }
  ],
  "isFeatured": false,
  "isHidden": false,
  "projectOrder": 7
}
```

### Change Theme Colors
**File:** `index.html` - Edit `:root` CSS variables:
```css
:root {
  --accent: #ff4500;  /* Change to new color */
  --bg: #faf9f7;
  /* ... */
}
```

### Add New Language
**File:** Any HTML - Edit i18n object in `<script>`:
```javascript
const i18n = {
  en: { /* ... */ },
  pt: {  // Portuguese
    'nav-home': 'Início',
    'nav-about': 'Sobre',
    /* ... */
  }
};
```

---

## Troubleshooting

| Issue | Solution |
|---|---|
| Content not updating | Clear browser cache, use `?t=` timestamp |
| Theme not persisting | Check localStorage enabled in browser |
| Images not loading | Verify Cloudinary URL is correct & public |
| Analytics not tracking | Check GA ID & Clarity project ID in HTML |
| Mobile menu not closing | Ensure event listeners bound (check console) |
| Page transitions janky | Verify CSS fade-out animation timing |

---

## Links & Resources

| Resource | Link |
|---|---|
| **GitHub Repo** | https://github.com/MawuliPRINCE/my-portfolio |
| **Live Site** | https://mawuliprince.github.io/my-portfolio/ |
| **Full Architecture** | See ARCHITECTURE.md |
| **Product Specs** | See PRD.md |
| **Google Analytics** | https://analytics.google.com (ID: G-RNQ0C2YB4D) |
| **Cloudinary** | https://cloudinary.com (CDN for images) |

---

## Key Metrics & Targets

```
Page Load Time:         < 2 seconds
Lighthouse Score:       90+
Mobile Traffic:         60%+
Bounce Rate:            < 50%
Average Session:        2+ minutes
Core Web Vitals:        All "Good"
```

---

## Next Steps for Development

### Phase 1 (Current)
- ✅ Portfolio showcase
- ✅ Contact information
- ✅ Multi-language support
- ✅ Dark/light theme

### Phase 2 (Planned)
- [ ] Contact form with backend
- [ ] Booking/consultation integration
- [ ] Blog section
- [ ] Client testimonials

### Phase 3 (Future)
- [ ] Headless CMS (Contentful)
- [ ] E-commerce for digital products
- [ ] Admin dashboard UI
- [ ] Advanced analytics

---

**For detailed technical information, see [ARCHITECTURE.md](./ARCHITECTURE.md)**

**For product & requirements details, see [PRD.md](./PRD.md)**

---

*Last Updated: September 2026*
*Quick Reference Guide v1.0*
