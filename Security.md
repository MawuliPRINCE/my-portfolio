# Security Documentation
## Ismael Asumanu Prince — Graphic Designer Portfolio

**Document Version:** 1.0  
**Last Updated:** September 2026  
**Status:** Active

---

## Overview

This document outlines the security posture, controls, risks, and recommended practices for the portfolio website. The site is a **static website** hosted on GitHub Pages with no traditional backend server. Security therefore focuses on:

- Secure delivery (HTTPS)
- Content Security Policy (CSP)
- Safe handling of external resources
- Protection of client inquiry data
- Prevention of common client-side vulnerabilities

---

## Security Model Summary

| Layer | Status | Notes |
|-------|--------|-------|
| **Transport Security** | Strong | GitHub Pages forces HTTPS + HSTS |
| **Content Security Policy** | Present | Defined via meta tag / headers |
| **Authentication** | None (public site) | Admin pages currently unprotected |
| **Authorization** | None | No user roles |
| **Data at Rest** | Git repository | Public by default |
| **Client Preferences** | localStorage only | Theme + language (non-sensitive) |
| **Third-party Scripts** | Controlled | Google Analytics, Microsoft Clarity, Font Awesome, Google Fonts |
| **Form Handling** | Basic | Bookings currently stored in public JSON |

---

## 1. Transport Security

- **HTTPS is enforced** by GitHub Pages.
- SSL/TLS certificates are automatically provisioned and renewed by GitHub.
- HSTS is enabled by default on GitHub Pages.
- All external resources (Cloudinary, Google Fonts, analytics) are loaded over HTTPS.

**Result:** All traffic between the user and the site is encrypted.

---

## 2. Content Security Policy (CSP)

A Content Security Policy is implemented to restrict which resources the browser is allowed to load. This significantly reduces the risk of XSS (Cross-Site Scripting) and data injection attacks.

### Current Policy (Recommended / Implemented)

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
media-src 'self' https://*.cloudinary.com;
```

### Purpose of Each Directive

| Directive | Purpose |
|-----------|---------|
| `default-src 'self'` | Fallback — only allow resources from the same origin |
| `script-src` | Allow necessary analytics and CDN scripts |
| `style-src` | Allow Google Fonts and Font Awesome styles |
| `img-src` / `media-src` | Allow Cloudinary images and videos |
| `connect-src` | Allow fetching JSON content and analytics beacons |

**Note:** `'unsafe-inline'` is currently required for some inline scripts and styles. Future improvements should move toward nonces or hashes to remove this exception.

---

## 3. Client-Side Storage

Only non-sensitive preference data is stored in the browser:

| Key | Value | Sensitivity |
|-----|-------|-------------|----------|
| `theme` | `light` / `dark` | None |
| `lang` | `en` / `fr` / `es` / `de` | None |

**Rules:**
- Never store personal information, tokens, or booking data in `localStorage` or `sessionStorage`.
- Prefer `localStorage` only for UI preferences.

---

## 4. Data Security

### Public Content
Bio, skills, project case studies, email, and social links are intentionally public and stored in plain JSON files in the repository.

### Semi-Sensitive Data (`bookings.json`)
Client inquiries (name, email, phone, message, etc.) are currently stored in `bookings.json` inside the public repository.

**Risk Level:** Medium  
**Recommendation:**  
- Move booking submissions to a private backend, serverless function, or protected database as soon as possible.  
- Until then, treat the file as potentially exposed and avoid storing highly sensitive information in the form.

### Media
All images and videos are hosted on Cloudinary under a public cloud. Only intentionally public assets should be uploaded.

---

## 5. Third-Party Services

| Service | Purpose | Data Shared | Risk Mitigation |
|---------|---------|-------------|-----------------|
| **Google Analytics 4** | Traffic analytics | Page views, device info, approximate location | Use GA4 privacy features; no PII intentionally sent |
| **Microsoft Clarity** | Session recordings & heatmaps | Session behavior | Review Clarity privacy settings |
| **Cloudinary** | Media CDN | Public media only | Use signed URLs in future if private media is needed |
| **Google Fonts** | Typography | Font requests | Loaded over HTTPS |
| **Font Awesome / jsDelivr** | Icons | Script/style requests | Loaded over HTTPS |
| **GitHub Raw CDN** | JSON content delivery | Public JSON files | Cache-busting only; no authentication required |

---

## 6. Admin / Studio Pages

The repository contains `studio.html` and `admin (1).html`.

**Current State:**  
These pages are publicly accessible. There is no authentication or access control.

**Risk:** Anyone who knows the URL can view the admin interface.

**Recommended Mitigations (in order of priority):**
1. Move admin functionality behind authentication (GitHub OAuth, simple password, or external auth provider).
2. Or restrict access via GitHub Pages private repository + authentication (if available).
3. At minimum, avoid exposing sensitive operations (editing bookings, etc.) without protection.

---

## 7. Common Web Vulnerabilities — Status

| Vulnerability | Status | Notes |
|---------------|--------|-------|
| **XSS (Cross-Site Scripting)** | Partially mitigated | CSP is in place. HTML content from JSON should be sanitized before rendering. |
| **CSRF** | Low risk | No stateful authenticated actions currently |
| **Clickjacking** | Mitigated | `frame-src` is restricted |
| **Mixed Content** | Mitigated | HTTPS enforced |
| **Sensitive Data Exposure** | Medium risk | `bookings.json` is public |
| **Insecure Dependencies** | Low | Minimal third-party code; mostly CDNs |
| **Open Redirects** | Low | Navigation is internal |

---

## 8. Security Best Practices Currently Followed

- HTTPS everywhere
- Content Security Policy implemented
- Minimal use of third-party scripts
- No sensitive secrets stored in the repository
- Cache-busting used only for content freshness (not for security)
- Public content is intentionally public
- Theme and language preferences kept client-side only

---

## 9. Recommended Security Improvements (Roadmap)

### High Priority
- [ ] Protect or remove public access to admin/studio pages
- [ ] Move `bookings.json` (and future form data) to a private backend or serverless function
- [ ] Sanitize all HTML content coming from JSON before injecting into the DOM
- [ ] Remove or reduce `'unsafe-inline'` in CSP by using nonces or hashes

### Medium Priority
- [ ] Add input validation and rate limiting on any future contact/booking forms
- [ ] Implement a proper privacy policy page (especially if using Clarity session recordings)
- [ ] Regularly review Google Analytics and Clarity data retention settings
- [ ] Add security headers where possible (`X-Content-Type-Options`, `Referrer-Policy`, etc.)

### Nice to Have
- [ ] Subresource Integrity (SRI) for third-party scripts
- [ ] Automated dependency / CDN monitoring
- [ ] Periodic security review of the CSP

---

## 10. Incident Response (Lightweight)

Because this is a static portfolio site, the main risks are:

1. **Unauthorized content changes** → Revert via Git
2. **Exposure of inquiry data** → Rotate any compromised contact details and move data to private storage
3. **Compromised third-party account** (Cloudinary, Analytics) → Revoke keys / regenerate tokens and update the site

**Immediate actions if something goes wrong:**
1. Review recent Git commits
2. Check Cloudinary and analytics dashboards for unusual activity
3. Update or remove compromised JSON content
4. Force a fresh deploy by pushing a small change

---

## Related Documents

- [Data.md](./Data.md) — Data model and storage details
- [ARCHITECTURE.md](./ARCHITECTURE.md) — Full technical architecture
- [ARCHITECTURE-ESSENTIALS.md](./ARCHITECTURE-ESSENTIALS.md) — Quick reference
- [PRD.md](./PRD.md) — Product requirements

---

*This is a living security document. It should be reviewed and updated whenever the architecture, third-party services, or data handling practices change.*
