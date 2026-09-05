# Product Requirements Document (PRD)
## Ismael Asumanu Prince — Graphic Designer Portfolio

**Document Version:** 1.0  
**Last Updated:** September 2026  
**Created By:** GitHub Copilot  
**Status:** Active

---

## Executive Summary

This document outlines the requirements and specifications for the personal portfolio website of Ismael Asumanu Prince, a Graphic Designer and Brand Identity Designer based in Ghana. The site is a modern, responsive single-page application (SPA) built with vanilla HTML, CSS, and JavaScript, showcasing the designer's work, expertise, and providing multiple contact channels.

**Primary Objective:** Establish a professional digital presence that communicates design excellence, attracts potential clients, and facilitates business inquiries through a clean, intentional design aesthetic.

---

## Product Overview

### Product Name
**Ismael Asumanu Prince — Graphic Designer Portfolio**

### Product Type
Personal Portfolio Website / Digital Business Card

### Primary Users
- Potential clients seeking design services
- Hiring managers and recruiters
- Design industry peers
- Brand strategy collaborators

### Key Value Proposition
- **For Clients:** A showcase of diverse, high-quality design work across branding, identity, and visual systems
- **For Designers:** A platform to demonstrate brand strategy expertise and design versatility
- **For Businesses:** Easy access to a designer's capabilities with direct contact pathways

---

## Core Features

### 1. Navigation & User Experience
**Status:** Active

#### 1.1 Header Navigation
- **Fixed, responsive header** with smooth scroll-triggered state changes
- **Sticky navigation bar** that transitions from transparent to semi-transparent with backdrop blur on scroll
- **Logo ("Prince")** in accent orange (ff4500) serving as home link
- **Language selector dropdown** (EN, FR, ES, DE)
- **Theme toggle button** (light/dark mode)
- **Responsive hamburger menu** for mobile devices

#### 1.2 Mobile Menu
- **Off-canvas slide-out menu** from right side
- **Touch gesture support** (swipe to close)
- **Keyboard support** (ESC to close)
- **Menu backdrop** with semi-transparent overlay
- **Navigation links:** Home, About, Projects, Contact

#### 1.3 Navigation Links
- **Home:** Hero section with personalized greeting
- **About:** Designer bio, education, experience, and skills
- **Projects:** Comprehensive portfolio gallery with case studies
- **Contact:** Footer section with social media links

---

### 2. Landing Page (Home)

**Status:** Active

#### 2.1 Hero Section
- **Full-viewport hero** with dynamic background (video or image from CMS)
- **Gradient overlay:** Dark linear gradient (0-72% opacity left to 10% right) for text readability
- **Key Elements:**
  - Animated eyebrow text: "Portfolio"
  - Personalized greeting (time-based): "Good morning/afternoon/evening, I'm *Ismael*"
  - Designer role: "Graphic Designer · Brand Identity Designer"
  - Brief introduction text
  - "Explore Works" call-to-action button
  - Scroll indicator line animation

#### 2.2 Hero Animations
- **Staggered rise-up animations** with cubic-bezier easing
- **Subtle bounce animation** on CTA button
- **Scroll indicator line** that animates from top as page loads
- **Page transition effect** on navigation (fade-out before loading next page)

#### 2.3 Footer/Contact Section
- **Two-column layout** (responsive to single column on mobile)
- **CTA Heading:** "Let's Connect"
- **Accent divider line**
- **Description text:** Customizable contact pitch
- **Social media icons:**
  - Email
  - LinkedIn
  - Dribbble
  - Instagram
  - Behance
- **Footer bottom:** Copyright year, back-to-top button

#### 2.4 Scroll-to-Top Button
- **Fixed position** (bottom-right corner)
- **Circular progress ring** indicating scroll position
- **Only visible** when scrolled 300px or more
- **Smooth scroll animation** to top
- **Hover effect** with subtle lift

---

### 3. About Page

**Status:** Active

#### 3.1 Page Structure
- **About Bio Section**
  - Short bio (for initial viewing)
  - Full bio (expandable)
  - Customizable via CMS (site-about.json)

- **Education**
  - **Degree:** Bachelor of Arts (B.A.) Graphic Design
  - **Institution:** University of Education, Winneba
  - **Location:** Winneba, Ghana
  - **Timeline:** 2023 – Present

- **Professional Experience**
  - **Title:** Freelance Graphic Designer and Brand Identity Designer
  - **Employment Type:** Self-Employed
  - **Start Date:** 2024 – Present
  - **Summary:** Customizable experience description via CMS

- **Design Skills**
  - Brand Identity Design
  - Logo Design
  - Typography
  - Layout Design
  - Packaging Design
  - Visual Communication
  - Art Direction
  - And more (expandable list)

- **Project Statistics**
  - **Total Projects:** 500+
  - **Experience Since:** 2023

#### 3.2 CMS Data Source
- **File:** `site-about.json`
- **Updatable fields:**
  - `aboutShort` / `aboutFull`
  - `eduShort` / `eduFull`
  - `workShort` / `workFull`
  - `skillsShort` / `skillsFull`
  - `expStart`
  - `projectsCount`
  - `reviews` (array for testimonials)

---

### 4. Projects Gallery

**Status:** Active

#### 4.1 Page Layout
- **Grid display** of project cards
- **Responsive columns** (adapts to screen size)
- **Project card elements:**
  - Cover image/thumbnail
  - Project title
  - Subtitle/category
  - Tag badges (design category/type)
  - Year completed
  - Click to expand view

#### 4.2 Featured Projects
The portfolio includes the following featured projects:

| Project ID | Title | Category | Year | Tags |
|---|---|---|---|---|
| proj_1781062806683 | MIKITA | Fine Jewelry Branding | 2026 | Luxury, Brand Identity, Logo, Minimalist |
| proj_1781319653778 | DRIPR | Product Design | 2026 | Packaging, Beverage, Modern Minimalist |
| proj_1781345437740 | TERRAYIELD FARMS | Agricultural Brand | 2026 | Brand Strategy, Logo, Minimalist |
| proj_1782055251567 | AETHERGRID ENERGY | Brand Identity System | 2026 | Energy Tech, System Design |
| proj_1780264723478 | CHOP BOX | F&B Branding | 2026 | Restaurant, Streetfood, Visual Identity |
| proj_1782148368605 | Girl Genius Foundation | Social Impact Campaign | 2026 | Mental Health, Social Advocacy |

#### 4.3 Project Details
Each project contains:
- **Title & Subtitle**
- **Detailed description** (HTML-formatted case study)
- **Media gallery** (images and videos)
- **Tags/Categories** (multiple design disciplines)
- **Year completed**
- **Cover image index** (for gallery display)
- **Metadata:**
  - Client testimonials (when available)
  - Project type (self-initiated or client work)
  - AI percentage (attribution tracking)
  - SEO metadata (title, description, image)
  - Hidden/archived projects (not displayed)
  - Recent/featured flags

#### 4.4 CMS Data Source
- **File:** `projects.json`
- **Data structure:** JSON array of project objects
- **Live update:** Fetch with cache-busting timestamp to reflect CMS changes

---

### 5. Theming & Personalization

#### 5.1 Dark/Light Mode
- **Toggle button** in header with lightbulb icon
- **CSS variables** for theme colors
- **Persistent storage** (localStorage)
- **System preference detection** (prefers-color-scheme)

**Light Mode (Default):**
- Background: #faf9f7 (light cream)
- Surface: #f2f0ec
- Text: #111 (nearly black)
- Accent: #ff4500 (orange-red)
- Border: #e0ddd8
- Footer: #0d0d0d (dark)

**Dark Mode:**
- Background: #0d0d0d (nearly black)
- Surface: #161616
- Text: #f0ede8 (off-white)
- Muted: #555
- Border: #2a2a2a
- Footer: #000

#### 5.2 Language Localization
- **Supported Languages:** English (EN), French (FR), Spanish (ES), German (DE)
- **i18n implementation:** data-i18n attributes with JavaScript translation object
- **Persistent storage:** Language preference saved to localStorage
- **Translatable elements:** Navigation, hero copy, button text, footer text

**Translation Keys:**
```
nav-home, nav-about, nav-projects, nav-contact
hero-eyebrow, hero-role, explore
footer-title, footer-desc
```

#### 5.3 Responsive Typography
- **Font family:** DM Sans (primary), Cormorant Garamond (headings)
- **Fluid sizing:** clamp() for viewport-responsive text
- **Hierarchy:** Clear visual distinction between headings, body, and metadata

---

### 6. Performance & Analytics

#### 6.1 Analytics & Tracking
- **Google Analytics 4** (GA-RNQ0C2YB4D)
- **Microsoft Clarity** (Session tracking: xbuxkrh18o)
- **Purpose:** Track user engagement, conversion funnels, traffic sources

#### 6.2 Security
- **Content Security Policy (CSP)** header configured
- **Allowed sources:**
  - Google Tag Manager, Clarity, JSDelivr (scripts)
  - Google Fonts, Cloudflare CDN (styles & fonts)
  - Cloudinary, GitHub raw content (images & media)
  - GitHub API, Booking handler API (external APIs)

#### 6.3 SEO Metadata
- **Open Graph tags** for social sharing
- **Meta descriptions** for search results
- **robots.txt** for search engine crawling
- **Per-project SEO fields:** Custom title, description, image

---

### 7. Contact & Social Integration

#### 7.1 Contact Methods
- **Email:** princeismaelasumanu@gmail.com
- **LinkedIn:** www.linkedin.com/in/prince-asumanu-54404b417
- **Dribbble:** https://dribbble.com/prince-asumanu
- **Instagram:** (currently empty, ready for integration)
- **Behance:** (currently empty, ready for integration)

#### 7.2 Social Icon Links
- All social icons in footer link to respective profiles
- Icons use Font Awesome 6.5.1 library
- Hover effects with accent color and lift animation
- Configurable via CMS (site-home.json)

---

## Technical Specifications

### Tech Stack
- **Frontend:** Vanilla HTML5, CSS3, JavaScript (ES6+)
- **Hosting:** GitHub Pages (https://mawuliprince.github.io/my-portfolio/)
- **CMS:** JSON-based content files (site-home.json, site-about.json, projects.json)
- **Media:** Cloudinary CDN for images and videos
- **APIs:** GitHub API (raw.githubusercontent.com), Google Analytics

### Browser Support
- **Modern browsers:** Chrome, Firefox, Safari, Edge (latest versions)
- **Responsive breakpoints:** Mobile (<600px), Tablet (600-1024px), Desktop (>1024px)
- **Features:** CSS Grid, Flexbox, CSS custom properties, IntersectionObserver API

### Performance Targets
- **Page load:** <2 seconds
- **Lighthouse scores:** 90+ across all categories
- **Image optimization:** Cloudinary auto-compression (q_auto, f_auto)
- **Caching:** Cache-busting timestamps on JSON fetches

---

## File Structure

```
my-portfolio/
├── index.html                 # Home page
├── about.html                 # About page
├── projects.html              # Projects gallery
├── studio.html                # Admin/studio page
├── booking.html               # Booking/consultation page
├── admin (1).html             # Admin interface
├── 404.html                   # Not found page
│
├── site-home.json             # Home page CMS data
├── site-about.json            # About page CMS data
├── projects.json              # Projects gallery data
├── bookings.json              # Bookings/inquiries data
│
├── robots.txt                 # SEO crawling rules
└── README.md                  # Documentation
```

---

## User Flows

### Flow 1: Potential Client Discovery
1. User lands on home page (index.html)
2. Reviews hero section and greeting
3. Clicks "Explore Works" → Projects page
4. Browses project gallery, clicks to view case studies
5. Scrolls to footer, clicks social/email link
6. Completes inquiry

### Flow 2: About Page Deep Dive
1. User navigates to About page
2. Reads bio, education, experience
3. Views skill list and project count (500+)
4. Returns to Projects or Contact

### Flow 3: Theme/Language Preference
1. User clicks theme toggle or language selector in header
2. Preference saved to localStorage
3. Page appearance updates immediately
4. Preference persists on return visits

### Flow 4: Mobile Navigation
1. User on mobile device opens menu (hamburger icon)
2. Menu slides in from right with backdrop overlay
3. User clicks a link to navigate
4. Menu closes automatically
5. Page transition fades and loads new content

---

## Content Requirements

### Persona: Ismael Asumanu Prince
- **Role:** Graphic Designer & Brand Identity Designer
- **Location:** Ghana
- **Specialties:** 
  - Brand Identity Design
  - Logo Design
  - Visual Systems
  - Packaging Design
  - Art Direction
- **Experience:** Started 2024, pursuing B.A. in Graphic Design (University of Education, Winneba)
- **Target Clients:** Startups, SMEs, luxury brands, social enterprises
- **Brand Voice:** Professional, creative, intentional, forward-thinking

### Required Content Assets
- Professional headshot (hero background video/image)
- 6+ detailed project case studies
- Bio (short & long versions)
- Education details
- Work experience descriptions
- Skills list
- Social media profiles

---

## Future Enhancements

### Phase 2 Features
- [ ] Blog section for design insights
- [ ] Newsletter signup integration
- [ ] Client testimonials with ratings
- [ ] Direct contact form (instead of email)
- [ ] Booking/consultation calendar integration
- [ ] Animation library (scroll animations, parallax)
- [ ] Multi-language blog content
- [ ] Search functionality for projects

### Phase 3 Features
- [ ] E-commerce integration (digital products)
- [ ] Member portal for ongoing clients
- [ ] Case study video embeds
- [ ] Interactive design tool showcase
- [ ] AI-powered project recommendation
- [ ] Accessibility enhancements (WCAG 2.1 AAA compliance)

---

## Success Metrics

### Key Performance Indicators (KPIs)
| Metric | Target | Current |
|--------|--------|---------|
| Monthly Visitors | 500+ | TBD |
| Project Page Views | 1000+ | TBD |
| Contact Click-through Rate | 10%+ | TBD |
| Social Media Traffic | 20% of total | TBD |
| Average Session Duration | 2+ minutes | TBD |
| Bounce Rate | <50% | TBD |
| Mobile Traffic Percentage | 60%+ | TBD |

---

## Maintenance & Governance

### Content Updates
- **CMS files** (site-home.json, site-about.json, projects.json) updated as work evolves
- **Social links** maintained in site-home.json
- **Projects** archived/hidden via `isHidden` flag when relevant
- **Blog/news:** To be added in future phases

### Technical Maintenance
- **Analytics review:** Monthly (Google Analytics, Clarity)
- **Performance audit:** Quarterly (Lighthouse, Core Web Vitals)
- **Security updates:** As needed (CSP, HTTPS, dependencies)
- **Browser compatibility:** Tested on latest 2 versions of major browsers

### Deployment
- **Host:** GitHub Pages
- **Branch:** main (auto-deploys on push)
- **Version control:** Git commits with descriptive messages

---

## Appendix

### A. Color Palette
```
Accent:           #ff4500 (OrangeRed)
Accent (dim):     rgba(255, 69, 0, 0.08/0.12)
Background:       #faf9f7 (Light Mode) / #0d0d0d (Dark Mode)
Surface:          #f2f0ec (Light Mode) / #161616 (Dark Mode)
Text:             #111 (Light Mode) / #f0ede8 (Dark Mode)
Muted:            #888 (Light Mode) / #555 (Dark Mode)
Border:           #e0ddd8 (Light Mode) / #2a2a2a (Dark Mode)
Footer:           #0d0d0d (Background) / #f0ede8 (Text)
```

### B. Typography Scale
```
Hero Name:        clamp(58px, 9.5vw, 110px)
Section Heading:  clamp(30px, 5vw, 50px)
Body Text:        1rem (16px)
Small Text:       0.72rem (11.5px)
```

### C. External Dependencies
- **Google Fonts:** Cormorant Garamond, DM Sans
- **Icon Library:** Font Awesome 6.5.1
- **Analytics:** Google Tag Manager, Microsoft Clarity
- **Media CDN:** Cloudinary
- **API:** GitHub API (for content fetching)

### D. Legal / Compliance
- **Privacy:** Aligned with Google Analytics and Microsoft Clarity terms
- **Accessibility:** Ongoing improvements toward WCAG 2.1 AA compliance
- **Copyright:** © [Year] Ismael Asumanu Prince

---

**End of Document**

---

*This PRD is a living document and will be updated as the portfolio evolves. Last reviewed: September 2026.*
