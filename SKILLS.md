# SKILLS.md
## Technical Skills & Capabilities for Coding Agents

**Document Version:** 1.0  
**Last Updated:** September 2026  
**Purpose:** Technical skills inventory for agents working on the portfolio codebase

---

## Agent Technical Stack

### Required Skills for Portfolio Agents

This document outlines the technical knowledge, tools, and capabilities needed for agents to effectively work on the my-portfolio codebase.

---

## Core Technologies

### 1. HTML5 ⭐⭐⭐⭐⭐

**Requirement Level:** Essential

**Skills Needed:**
- Semantic HTML structure
- Meta tags (SEO, Open Graph, viewport)
- Form handling basics
- Accessibility attributes (ARIA labels, roles)
- HTML template patterns
- Document structure optimization

**Portfolio Usage:**
- 7 HTML pages (index.html, about.html, projects.html, etc.)
- Embedded CSS and JavaScript
- Meta tag management for SEO
- Accessibility compliance

**Agent Capabilities:**
- ✅ Read and modify HTML structure
- ✅ Add new sections/pages
- ✅ Update meta tags
- ✅ Implement accessibility improvements
- ✅ Validate semantic HTML
- ✅ Fix broken links/references

---

### 2. CSS3 ⭐⭐⭐⭐⭐

**Requirement Level:** Essential

**Skills Needed:**
- CSS custom properties (variables)
- Flexbox and CSS Grid layouts
- Media queries and responsive design
- CSS animations and transitions
- Pseudo-elements and pseudo-classes
- CSS cascade and specificity
- Mobile-first approach
- Viewport units (clamp, vw, vh)

**Portfolio Usage:**
- Inline `<style>` block in HTML
- Theme variables for dark/light mode
- Responsive breakpoints (600px, 1024px)
- Complex animations (scroll reveal, page transitions)
- Gradient overlays and backdrops

**Agent Capabilities:**
- ✅ Modify color schemes
- ✅ Adjust responsive breakpoints
- ✅ Create/update animations
- ✅ Improve accessibility (contrast ratios)
- ✅ Optimize CSS performance
- ✅ Add new component styles

**Common Tasks:**
```css
/* Theme variable updates */
:root { --accent: #ff4500; }

/* Responsive design changes */
@media (max-width: 600px) { /* mobile styles */ }

/* Animation creation */
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
```

---

### 3. JavaScript (ES6+) ⭐⭐⭐⭐⭐

**Requirement Level:** Essential

**Skills Needed:**
- Vanilla JavaScript (no framework)
- DOM manipulation (querySelector, classList, innerHTML)
- Event listeners and handlers
- Async/await and promises
- JSON parsing and stringification
- LocalStorage API
- Intersection Observer API
- Fetch API

**Portfolio JavaScript Modules:**

#### 3.1 Theme Management
```javascript
// Toggle dark/light mode
function toggleTheme()
document.body.classList.toggle('dark')
localStorage.setItem('theme', isDark ? 'dark' : 'light')
```

#### 3.2 Navigation & Menu
```javascript
// Mobile menu toggle
function openMenu()
function closeMenu()
function toggleMenu()

// Touch gesture support
menuEl.addEventListener('touchend', ...)

// Keyboard support (ESC key)
document.addEventListener('keydown', ...)
```

#### 3.3 Header Scroll State
```javascript
// Sticky header on scroll
function handleScroll()
if (window.scrollY > 60) { /* add scrolled class */ }
```

#### 3.4 Internationalization (i18n)
```javascript
// Language switching
const i18n = { en: {...}, fr: {...}, es: {...}, de: {...} }
function switchLang(lang)
function applyLang(lang)
document.querySelectorAll('[data-i18n]').forEach(...)
```

#### 3.5 Scroll Reveal & Animations
```javascript
// Intersection Observer for lazy animations
const scrollRevealObserver = new IntersectionObserver(...)
```

#### 3.6 Page Transitions
```javascript
// Fade between pages
document.body.classList.add('fade-out')
setTimeout(() => { window.location = href }, 300)
```

#### 3.7 CMS Data Fetching
```javascript
// Fetch from GitHub raw CDN with cache-busting
fetch(url + '?t=' + Date.now())
  .then(r => r.ok ? r.json() : null)
  .then(d => { /* inject into DOM */ })
  .catch(() => { /* use defaults */ })
```

**Agent Capabilities:**
- ✅ Add new JavaScript modules
- ✅ Debug existing code
- ✅ Implement new features (animations, interactions)
- ✅ Optimize performance
- ✅ Add event listeners
- ✅ Modify CMS data fetching logic

---

### 4. JSON ⭐⭐⭐⭐⭐

**Requirement Level:** Essential

**Skills Needed:**
- JSON syntax and validation
- Nested object structures
- Array handling
- JSON parsing/stringification
- Schema validation
- Data structure design

**Portfolio JSON Files:**

#### 4.1 site-home.json
```json
{
  "name": "string",
  "role": "string",
  "heroBg": "url",
  "contactDesc": "string",
  "email": "string",
  "linkedin": "url",
  "dribbble": "url",
  "instagram": "url",
  "behance": "url"
}
```

#### 4.2 site-about.json
```json
{
  "aboutShort": "string",
  "aboutFull": "string|html",
  "eduShort": "string",
  "eduFull": "string|html",
  "workShort": "string",
  "workFull": "string|html",
  "skillsShort": "string",
  "skillsFull": "string|html",
  "reviews": "array",
  "expStart": "string",
  "projectsCount": "string"
}
```

#### 4.3 projects.json
```json
[
  {
    "id": "string",
    "title": "string",
    "subtitle": "string",
    "description": "string|html",
    "tags": ["string"],
    "year": "string",
    "media": [
      { "url": "string", "type": "image|video" }
    ],
    "isFeatured": "boolean",
    "isHidden": "boolean",
    "projectOrder": "number"
  }
]
```

**Agent Capabilities:**
- ✅ Validate JSON structure
- ✅ Add/remove properties
- ✅ Modify values
- ✅ Fix JSON syntax errors
- ✅ Migrate data between versions
- ✅ Generate new JSON from templates

---

## Framework & Library Skills

### 1. No Framework (Vanilla Stack) ⭐⭐⭐⭐⭐

**Importance:** Critical

The portfolio uses **zero JavaScript frameworks** (no React, Vue, etc.). This means:
- Lightweight and fast loading
- No build process required
- Direct DOM manipulation
- Simple to understand and modify
- Easy for agents to work with

**Agent Advantage:** Simple codebase = easier analysis and modifications

---

### 2. Font Awesome (Icon Library) ⭐⭐⭐

**CDN Link:** `https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css`

**Usage:**
```html
<i class="fas fa-lightbulb"></i>  <!-- Theme toggle -->
<i class="fas fa-bars"></i>       <!-- Menu toggle -->
<i class="fab fa-linkedin-in"></i> <!-- Social icons -->
```

**Agent Tasks:**
- ✅ Add new icons to pages
- ✅ Update icon classes
- ✅ Find appropriate icons for new features

---

### 3. Google Fonts ⭐⭐⭐

**Fonts Used:**
- Cormorant Garamond (headings - serif, elegant)
- DM Sans (body text - sans-serif, modern)

**CSS Import:**
```css
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;600&family=DM+Sans:wght@300;400;500&display=swap');
```

**Agent Tasks:**
- ✅ Change font selections
- ✅ Adjust font weights
- ✅ Add new font variants

---

## External APIs & Services

### 1. Google Analytics 4 ⭐⭐⭐

**Tracking ID:** `G-RNQ0C2YB4D`

**Integration:**
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-RNQ0C2YB4D"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-RNQ0C2YB4D');
</script>
```

**Agent Tasks:**
- ✅ Update tracking ID
- ✅ Add custom event tracking
- ✅ Modify analytics configuration
- ✅ Verify tracking implementation

---

### 2. Microsoft Clarity ⭐⭐⭐

**Project ID:** `xbuxkrh18o`

**Integration:**
```javascript
(function(c,l,a,r,i,t,y){
  c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
  t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
  // ...
})(window, document, "clarity", "script", "xbuxkrh18o");
```

**Agent Tasks:**
- ✅ Update Clarity project ID
- ✅ Verify tracking implementation

---

### 3. GitHub API ⭐⭐⭐⭐

**Endpoint:** `https://raw.githubusercontent.com/MawuliPRINCE/my-portfolio/main/`

**Usage:**
```javascript
// Fetch JSON data with cache-busting
const url = 'https://raw.githubusercontent.com/.../site-home.json?t=' + Date.now();
fetch(url)
  .then(r => r.ok ? r.json() : null)
  .then(data => { /* inject into DOM */ })
```

**Agent Tasks:**
- ✅ Understand URL structure
- ✅ Handle fetch errors gracefully
- ✅ Implement cache-busting timestamps
- ✅ Validate API responses

---

### 4. Cloudinary (Image CDN) ⭐⭐⭐⭐

**Purpose:** Image and video hosting/optimization

**URL Pattern:**
```
https://res.cloudinary.com/dtjahgffs/image/upload/v{timestamp}/{image-id}.{format}
https://res.cloudinary.com/dtjahgffs/image/upload/q_auto,f_auto/{image-id}.jpg
```

**Auto-optimization Parameters:**
- `q_auto` - Automatic quality optimization
- `f_auto` - Automatic format selection (WebP, JPEG, etc.)

**Agent Tasks:**
- ✅ Validate Cloudinary URLs
- ✅ Optimize URLs with auto-compression
- ✅ Convert images to Cloudinary format
- ✅ Implement responsive image handling

---

## Version Control & Deployment

### 1. Git & GitHub ⭐⭐⭐⭐⭐

**Repository:** `MawuliPRINCE/my-portfolio`  
**Default Branch:** `main`  
**Hosting:** GitHub Pages

**Key Workflows:**
```bash
# Pull latest code
git fetch origin main
git pull origin main

# Create feature branch
git checkout -b feature/new-feature

# Commit changes
git add .
git commit -m "Description of changes"

# Push to remote
git push origin feature/new-feature

# Create pull request (optional)
# Merge to main triggers auto-deployment
```

**Agent Capabilities:**
- ✅ Read file contents from repo
- ✅ Understand Git commit structure
- ✅ Create commits with descriptive messages
- ✅ Push changes to repository
- ✅ Understand GitHub workflow

---

### 2. GitHub Pages ⭐⭐⭐

**URL:** `https://mawuliprince.github.io/my-portfolio/`

**Deployment:**
- Push to `main` branch → Auto-deploys in ~1 minute
- No build process required
- Static file serving
- HTTPS enforced
- CDN-backed global delivery

**Agent Understanding:**
- ✅ Know site is live in ~60 seconds after commit
- ✅ Understand no build step needed
- ✅ Know all files in repo become public

---

### 3. GitHub Actions (CI/CD) ⭐⭐⭐

**Optional Workflow Setup:**
```yaml
name: Validation
on: [push, pull_request]
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run validation
        run: node scripts/validate.js
```

**Agent Tasks:**
- ✅ Understand workflow structure
- ✅ Create validation workflows
- ✅ Run automated tests/checks

---

## Content Security & Standards

### 1. Content Security Policy (CSP) ⭐⭐⭐⭐

**Meta Tag:**
```html
<meta http-equiv="Content-Security-Policy" content="
  default-src 'self';
  script-src 'self' 'unsafe-inline' https://www.googletagmanager.com https://cdn.jsdelivr.net https://www.clarity.ms;
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com https://cdnjs.cloudflare.com;
  img-src 'self' data: https://*.cloudinary.com https://raw.githubusercontent.com;
  font-src 'self' https://fonts.gstatic.com https://cdnjs.cloudflare.com;
  connect-src 'self' https://raw.githubusercontent.com https://api.github.com https://www.google-analytics.com;
">
```

**Agent Tasks:**
- ✅ Understand CSP restrictions
- ✅ Know what domains are whitelisted
- ✅ Update CSP when adding new external resources
- ✅ Ensure new features comply with CSP

---

### 2. SEO Best Practices ⭐⭐⭐⭐

**Implementation:**
- Meta descriptions
- Open Graph tags
- Structured data (JSON-LD)
- robots.txt
- Sitemap.xml (optional)
- Canonical URLs
- Mobile-friendly design

**Agent Tasks:**
- ✅ Update meta tags
- ✅ Generate SEO metadata
- ✅ Optimize for search engines
- ✅ Test SEO compliance

---

### 3. Accessibility (a11y) ⭐⭐⭐⭐

**Standards:** WCAG 2.1 AA

**Implementation:**
- Semantic HTML
- ARIA labels and roles
- Color contrast ratios
- Keyboard navigation
- Screen reader support
- Alt text for images

**Agent Tasks:**
- ✅ Add ARIA attributes
- ✅ Improve color contrast
- ✅ Test keyboard navigation
- ✅ Generate alt text

---

## Performance Optimization

### 1. Lazy Loading ⭐⭐⭐

**Images:**
```html
<img src="..." loading="lazy" />
```

**JavaScript (Intersection Observer):**
```javascript
const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      // Load or activate element
    }
  });
});
```

**Agent Tasks:**
- ✅ Implement lazy loading
- ✅ Optimize image loading
- ✅ Improve Core Web Vitals

---

### 2. Caching Strategy ⭐⭐⭐

```
Static Assets (HTML/CSS/JS):
- Cached by browser (~1 hour)
- Cached by GitHub CDN

JSON CMS Data:
- Cache-busted with ?t=Date.now()
- Ensures fresh content on each visit

Cloudinary Media:
- Long-term CDN caching (1 year)
- Immutable URLs
```

**Agent Tasks:**
- ✅ Implement cache-busting
- ✅ Understand caching layers
- ✅ Optimize cache headers

---

### 3. Bundle Size & Optimization ⭐⭐⭐

**Current:** ~100KB total (without images)
- Inline CSS (no separate stylesheet)
- Inline JavaScript (no external libraries)
- Minimal external dependencies

**Agent Tasks:**
- ✅ Keep bundle small
- ✅ Avoid heavy libraries
- ✅ Optimize code efficiency

---

## Browser & Device Support

### Target Browsers
- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)

### Mobile Support
- iOS Safari
- Android Chrome
- Responsive design (fluid, not breakpoint-heavy)

### Agent Capabilities:**
- ✅ Test cross-browser compatibility
- ✅ Ensure mobile responsiveness
- ✅ Use modern CSS/JS features
- ✅ Provide fallbacks for older browsers

---

## Testing & Quality Assurance

### 1. HTML Validation ⭐⭐⭐

Tool: W3C HTML Validator
```bash
# Validate HTML files
https://validator.w3.org/
```

**Agent Tasks:**
- ✅ Validate semantic structure
- ✅ Check for broken HTML
- ✅ Ensure valid attribute usage

---

### 2. CSS Validation ⭐⭐⭐

Tool: W3C CSS Validator
```bash
# Validate CSS
https://jigsaw.w3.org/css-validator/
```

**Agent Tasks:**
- ✅ Check CSS syntax
- ✅ Verify vendor prefixes
- ✅ Optimize selectors

---

### 3. Performance Testing ⭐⭐⭐⭐

Tool: Google Lighthouse, PageSpeed Insights
```bash
# Key metrics:
- Largest Contentful Paint (LCP): < 2.5s
- First Input Delay (FID): < 100ms
- Cumulative Layout Shift (CLS): < 0.1
- Performance Score: 90+
```

**Agent Tasks:**
- ✅ Run Lighthouse audits
- ✅ Optimize Core Web Vitals
- ✅ Monitor performance trends

---

### 4. SEO Testing ⭐⭐⭐

Tool: Screaming Frog, SEO tools

**Agent Tasks:**
- ✅ Check meta tags
- ✅ Verify structured data
- ✅ Test Open Graph tags
- ✅ Validate mobile friendliness

---

### 5. Accessibility Testing ⭐⭐⭐⭐

Tool: WAVE, Axe DevTools, Screen Reader testing

**Agent Tasks:**
- ✅ Run accessibility audits
- ✅ Test keyboard navigation
- ✅ Verify color contrast
- ✅ Test screen reader compatibility

---

## Documentation Skills

### 1. Markdown ⭐⭐⭐⭐

**Files in repo:**
- README.md
- ARCHITECTURE.md
- PRD.md
- AGENTS.md
- CLAUDE.md
- SKILLS.md
- DATA.md
- SECURITY.md

**Agent Tasks:**
- ✅ Write/edit documentation
- ✅ Format with Markdown syntax
- ✅ Maintain documentation structure
- ✅ Link between documents

---

### 2. Code Comments ⭐⭐⭐

**Style:**
```javascript
// Single line comments for brief explanations
/* Multi-line comments for longer descriptions
   and context about why code exists */
```

**Agent Tasks:**
- ✅ Add clear comments
- ✅ Document complex logic
- ✅ Maintain code readability

---

## Debugging & Troubleshooting

### Browser DevTools ⭐⭐⭐⭐

**Skills:**
- Inspect HTML/CSS
- Debug JavaScript
- View Network requests
- Check Console for errors
- Monitor Performance
- Test Responsive design
- Validate CSP headers

**Agent Tasks:**
- ✅ Identify bugs using DevTools
- ✅ Debug JavaScript errors
- ✅ Check network requests
- ✅ Verify security policies

---

### Common Issues & Solutions

| Issue | Debugging Approach |
|---|---|
| Page not loading styles | Check CSS in DevTools, verify CSP |
| JavaScript not running | Check Console errors, verify script paths |
| Images not displaying | Check Network tab, verify Cloudinary URLs |
| Data not fetching | Check Network tab, verify GitHub URLs |
| Mobile layout broken | Check viewport meta tag, test responsive design |
| Analytics not tracking | Verify GA ID, check Console for errors |

---

## Skill Progression Path for Agents

### Level 1: Basic (Read & Understand)
- ✅ Read HTML structure
- ✅ Understand CSS styling
- ✅ Parse JavaScript logic
- ✅ Read JSON data

### Level 2: Intermediate (Modify & Update)
- ✅ Edit HTML content
- ✅ Update CSS values
- ✅ Modify JavaScript logic
- ✅ Add/remove JSON properties

### Level 3: Advanced (Create & Optimize)
- ✅ Create new pages/components
- ✅ Build CSS animations
- ✅ Write new JavaScript modules
- ✅ Design new data structures
- ✅ Optimize performance
- ✅ Implement accessibility features

### Level 4: Expert (Strategy & Architecture)
- ✅ Plan major features
- ✅ Architect new systems
- ✅ Lead refactoring efforts
- ✅ Establish best practices
- ✅ Mentor other agents

---

## Tools & Resources

### Online Tools
- **HTML Validator:** https://validator.w3.org/
- **CSS Validator:** https://jigsaw.w3.org/css-validator/
- **Lighthouse:** https://developers.google.com/web/tools/lighthouse
- **SEO Checker:** https://seotesteronline.com/
- **Color Contrast:** https://webaim.org/resources/contrastchecker/

### Documentation
- **MDN Web Docs:** https://developer.mozilla.org/
- **CSS-Tricks:** https://css-tricks.com/
- **JavaScript Info:** https://javascript.info/
- **GitHub Docs:** https://docs.github.com/

---

## Skill Assessment Checklist

Before working on the portfolio, agents should verify they can:

- [ ] Read and understand HTML semantics
- [ ] Write and debug CSS
- [ ] Understand vanilla JavaScript patterns
- [ ] Parse and modify JSON
- [ ] Use Git and GitHub
- [ ] Understand responsive design
- [ ] Know Web APIs (fetch, localStorage, etc.)
- [ ] Understand accessibility principles
- [ ] Optimize web performance
- [ ] Debug with browser DevTools

---

*Last Updated: September 2026*  
*Technical Skills Guide v1.0*
