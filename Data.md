# Data Documentation
## Ismael Asumanu Prince — Graphic Designer Portfolio

**Document Version:** 1.0  
**Last Updated:** September 2026  
**Status:** Active

---

## Overview

This document describes the data architecture, storage strategy, content model, and data flow of the portfolio website. The site uses a **lightweight, file-based CMS** built entirely on static JSON files. There is no traditional database or backend server.

**Core Principle:** All content is version-controlled in Git, served statically via GitHub Pages / GitHub Raw CDN, and injected into the page at runtime using vanilla JavaScript.

---

## Data Storage Strategy

| Aspect | Implementation |
|--------|----------------|
| **Storage Type** | Static JSON files in the repository |
| **Hosting** | GitHub (raw.githubusercontent.com CDN) |
| **Version Control** | Full Git history |
| **Caching** | Cache-busted with `?t=Date.now()` on every fetch |
| **Fallback** | Hardcoded default values in HTML/JS if fetch fails |
| **Media** | Cloudinary CDN (images & videos) |
| **User Preferences** | Browser `localStorage` (theme + language only) |
| **Form Submissions** | Stored in `bookings.json` (currently manual / client-side) |

---

## Data Files

### 1. `site-home.json`
**Purpose:** Controls the Home page (Hero section + Footer/Contact)

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Full name displayed in the hero |
| `role` | string | Professional title / role |
| `heroBg` | string (URL) | Background video or image for the hero |
| `contactDesc` | string | Optional description text in the contact section |
| `email` | string | Contact email |
| `linkedin` | string (URL) | LinkedIn profile |
| `dribbble` | string (URL) | Dribbble profile |
| `instagram` | string (URL) | Instagram profile (optional) |
| `behance` | string (URL) | Behance profile (optional) |

**Used by:** `index.html`

---

### 2. `site-about.json`
**Purpose:** Controls the About page content

| Field | Type | Description |
|-------|------|-------------|
| `aboutShort` | string | Short bio (collapsed view) |
| `aboutFull` | string / HTML | Full bio (expandable) |
| `eduShort` | string | Short education summary |
| `eduFull` | string / HTML | Full education details |
| `workShort` | string | Short work experience |
| `workFull` | string / HTML | Full work experience |
| `skillsShort` | string | Short skills list |
| `skillsFull` | string / HTML | Full skills (Design + Software + Soft skills) |
| `reviews` | array | Testimonials (currently empty) |
| `expStart` | string | Year experience started (e.g. `"2023"`) |
| `projectsCount` | string | Displayed project count (e.g. `"500+"`) |

**Used by:** `about.html`

---

### 3. `projects.json`
**Purpose:** Portfolio projects data (array of project objects)

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique project ID (e.g. `proj_1782148368605`) |
| `title` | string | Project title |
| `subtitle` | string | Category / short description |
| `description` | string / HTML | Full case study content |
| `tags` | array of strings | Design categories / tags |
| `year` | string | Year completed |
| `media` | array of objects | Gallery items (`url`, `type`, `showInRecents`) |
| `collaborators` | array of strings | Team members (if any) |
| `isRecent` | boolean | Show in "Recent" section |
| `isFeatured` | boolean | Highlight as featured |
| `coverIndex` | number | Which media item is the cover |
| `aiPercent` | number | AI contribution percentage (0–100) |
| `projectOrder` | number | Manual sort order |
| `recentOrder` | number | Order in Recent section |
| `isHidden` | boolean | Hide from public gallery |
| `seoTitle` | string | Custom SEO title |
| `seoDescription` | string | Custom SEO description |
| `seoImage` | string (URL) | Custom Open Graph image |
| `projectType` | string | Client work / Self-initiated |
| `clientTestimonial` | string | Client quote |

**Used by:** `projects.html`, potentially home page recent works

---

### 4. `bookings.json`
**Purpose:** Stores consultation / inquiry submissions

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique booking ID |
| `date` | string (ISO) | Submission timestamp |
| `name` | string | Client name |
| `email` | string | Client email |
| `phone` | string | Phone number (optional) |
| `company` | string | Company name |
| `projectType` | string | Type of project |
| `budget` | string | Budget range |
| `timeline` | string | Preferred timeline |
| `message` | string | Free-text message |
| `read` | boolean | Whether the inquiry has been read |
| `replied` | boolean | Whether a reply has been sent |

**Used by:** `booking.html` + `studio.html` / admin interface

---

## Data Flow

### Content Loading (Public Site)
```
1. User visits a page (index.html / about.html / projects.html)
2. JavaScript constructs URL:
   https://raw.githubusercontent.com/MawuliPRINCE/my-portfolio/main/{file}.json?t={timestamp}
3. Fetch request is made
4. On success → JSON is parsed and injected into the DOM
5. On failure → Hardcoded fallback content is used
```

### Content Updates (Admin / Manual)
```
1. Edit the relevant JSON file (via GitHub UI, Studio page, or local commit)
2. Commit & push to main branch
3. GitHub Pages + Raw CDN update (usually < 1 minute)
4. Next visitor receives fresh data (thanks to cache-busting)
```

### User Preferences (Client-side only)
```
localStorage keys:
- theme → "light" | "dark"
- lang  → "en" | "fr" | "es" | "de"
```

These preferences never leave the user’s browser.

---

## Media Data

All project images and the hero video are hosted on **Cloudinary**:

- Cloud name: `dtjahgffs`
- Automatic optimization: `q_auto,f_auto`
- Supports both images and videos
- Long-term caching + global CDN

Media objects in `projects.json` follow this structure:
```json
{
  "url": "https://res.cloudinary.com/dtjahgffs/...",
  "type": "image" | "video",
  "showInRecents": true | false
}
```

---

## Data Ownership & Sensitivity

| Data Type | Sensitivity | Location | Notes |
|-----------|-------------|----------|-------|
| Bio, skills, education | Public | `site-about.json` | Intentionally public |
| Project case studies | Public | `projects.json` | Portfolio content |
| Contact email & socials | Public | `site-home.json` | Intentionally public |
| Booking submissions | **Semi-sensitive** | `bookings.json` | Contains personal contact info |
| Theme / Language | Non-sensitive | localStorage | Preference only |
| Analytics | Aggregated | Google Analytics / Clarity | No PII stored on site |

**Important:** `bookings.json` currently lives in the public repository. Future improvements should move inquiry data to a private backend or protected storage.

---

## Data Integrity Rules

1. **All public content must be intentionally public.**
2. HTML content inside JSON (`description`, `aboutFull`, etc.) should be sanitized before rendering in future versions.
3. Project IDs should remain unique (`proj_` + timestamp pattern is recommended).
4. Setting `isHidden: true` removes a project from the public gallery without deleting it.
5. Cache-busting (`?t=Date.now()`) is mandatory on all JSON fetches to avoid stale content.

---

## Future Data Considerations

| Improvement | Benefit |
|-------------|---------|
| Move `bookings.json` to a private backend / database | Better privacy for client inquiries |
| Add input validation + sanitization on forms | Prevent XSS and malformed data |
| Introduce a proper headless CMS (Contentful, Sanity, etc.) | Easier non-technical content editing |
| Add schema validation for JSON files | Catch broken content early |
| Separate public vs private data more clearly | Stronger security posture |

---

## Related Documents

- [ARCHITECTURE.md](./ARCHITECTURE.md) — Full system architecture
- [ARCHITECTURE-ESSENTIALS.md](./ARCHITECTURE-ESSENTIALS.md) — Quick reference
- [PRD.md](./PRD.md) — Product requirements
- [Security.md](./Security.md) — Security policies and controls

---

*This document is a living reference and should be updated whenever the data model changes.*
