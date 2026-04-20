# Issue Log — dineshlog.github.io Portfolio

> GitHub-compatible issue list. Each entry maps 1:1 to a GitHub Issue.
> Labels: `bug` `enhancement` `content` `ux` `seo` `a11y` `performance`
> Priority: 🔴 Critical · 🟠 High · 🟡 Medium · 🟢 Low

---

## Open Issues

---

### #001 · 🔴 Bug — Scroll-spy nav highlight broken in dark mode
**Labels:** `bug` `ux`
**File:** `src/components/Nav.astro`

**Description:**
The IntersectionObserver scroll-spy script calls
`classList.toggle('dark:text-blue-400', bool)` to highlight the active nav link.
CSS class names containing `:` (Tailwind dark-mode variants) cannot be reliably
toggled via `classList` because the browser and CSS specificity handle them
differently from plain class names. The highlight works in light mode but the
dark-mode colour variant does not activate correctly.

**Steps to reproduce:**
1. Open the site in dark mode
2. Scroll through sections
3. Active nav link does not turn blue

**Acceptance criteria:**
- Active nav link highlights correctly in both light and dark mode
- Replace `dark:text-blue-400` toggle with a simple `.nav-active` CSS class
  defined in both light and dark contexts via Tailwind `@layer`

---

### #002 · 🔴 Bug — `robots.txt` references wrong sitemap filename
**Labels:** `bug` `seo`
**File:** `public/robots.txt`

**Description:**
`robots.txt` declares `Sitemap: https://dineshlog.github.io/sitemap.xml`
but `@astrojs/sitemap` generates `sitemap-index.xml` (with a sub-file
`sitemap-0.xml`). Google Search Console and crawlers will follow the
declared path, find a 404, and never discover the sitemap.

**Current:**
```
Sitemap: https://dineshlog.github.io/sitemap.xml
```
**Expected:**
```
Sitemap: https://dineshlog.github.io/sitemap-index.xml
```

**Acceptance criteria:**
- `robots.txt` points to `sitemap-index.xml`
- Verified in Google Search Console after deployment

---

### #003 · 🟠 Bug — All certification badge images are missing (404)
**Labels:** `bug` `content`
**File:** `public/badges/` (empty)

**Description:**
Every `<img src="/badges/sc-401.png">` returns 404. The `onerror` handler
falls back to a generic emoji (🏅), which looks unprofessional in the
Certifications section. All 20+ badge image references have no files behind them.

**Required badge files (sourced from Credly / Microsoft Learn):**
| File | Cert |
|---|---|
| `sc-401.png` | Information Security Administrator |
| `m365-admin-expert.png` | M365 Administrator Expert |
| `endpoint-admin.png` | Endpoint Administrator |
| `sc-300.png` | Identity and Access Administrator |
| `ws-hybrid.png` | Windows Server Hybrid Admin |
| `copilot-admin.png` | Copilot & Agent Administration |
| `sc-900.png` | Security, Compliance & Identity Fundamentals |
| `isc2-cc.png` | ISC2 Certified in Cybersecurity |
| `aws-saa.png` | AWS Solutions Architect Associate |
| `applied-xdr.png` | Applied: Defender XDR |
| `applied-purview.png` | Applied: Purview DLP |
| `applied-adds.png` | Applied: ADDS |
| `azure-admin.png` | Azure Administrator (historical) |
| `messaging-admin.png` | Messaging Admin (historical) |
| `modern-desktop.png` | Modern Desktop Admin (historical) |
| `mct.png` | Microsoft Certified Trainer (historical) |
| `mcse-cloud.png` | MCSE Cloud Platform (historical) |
| `mcse-productivity.png` | MCSE Productivity (historical) |
| `mcse-mobility.png` | MCSE Mobility (historical) |
| `mcsa-ws2016.png` | MCSA Windows Server 2016 (historical) |
| `mcsa-ws2012.png` | MCSA Windows Server 2012 (historical) |
| `mcsa-o365.png` | MCSA Office 365 (historical) |
| `mcsa-w10.png` | MCSA Windows 10 (historical) |
| `mcp.png` | Microsoft Certified Professional (historical) |

**How to get them:**
- Download PNG badges from [credly.com/users/loganathan-dinesh/badges](https://credly.com/users/loganathan-dinesh/badges)
- Download from [learn.microsoft.com/en-us/users/logan-dinesh/transcript](https://learn.microsoft.com/en-us/users/logan-dinesh/transcript)
- Place each file in `public/badges/` with the exact filename above

**Acceptance criteria:**
- All active cert cards show real badge images
- Historical certs show real badge images (or graceful greyscale fallback)
- No broken-image emoji visible anywhere in the section

---

### #004 · 🟠 Bug — OG image is SVG; unsupported by Twitter/X and WhatsApp
**Labels:** `bug` `seo`
**File:** `public/og-image.svg`, `src/layouts/Layout.astro`

**Description:**
`og:image` points to `og-image.svg`. Twitter/X, WhatsApp, and several other
platforms require a raster image (PNG or JPEG) for link previews. SVG is not
rendered by their crawlers, resulting in blank previews when the portfolio URL
is shared.

**Recommended fix:**
- Create `public/og-image.png` (1200×630px) — a screenshot or designed image
- Update `og:image` in `Layout.astro` to reference the PNG
- Add explicit dimensions: `og:image:width` = 1200, `og:image:height` = 630
- Keep SVG as a fallback or remove it

**Acceptance criteria:**
- Link preview shows a branded image when URL is shared on LinkedIn, Twitter/X, WhatsApp
- Validate with [opengraph.xyz](https://www.opengraph.xyz)

---

### #005 · 🟠 Enhancement — Replace initials avatar with real professional headshot
**Labels:** `enhancement` `content`
**Files:** `src/components/Hero.astro`, `src/components/About.astro`

**Description:**
The Hero section currently shows a gradient circle with "DL" initials as a
placeholder avatar. A professional headshot significantly increases recruiter
trust and personal connection on a portfolio site.

**Acceptance criteria:**
- Add `public/profile.jpg` (or `.png`) — a professional square headshot, min 400×400px
- Replace the DL gradient circle in `Hero.astro` with `<img src="/profile.jpg" alt="Dinesh Loganathan" />`
- Apply `rounded-full object-cover` classes and maintain the green online dot
- Ensure image is optimised (≤150 KB) — use [Squoosh](https://squoosh.app)

---

### #006 · 🟡 Enhancement — Add real LinkedIn recommendations to Testimonials
**Labels:** `enhancement` `content`
**File:** `src/components/Testimonials.astro`

**Description:**
The Testimonials section currently redirects visitors to LinkedIn rather than
showing actual recommendations. Displaying real quotes from colleagues and
managers directly on the portfolio is significantly more impactful.

**How to get them:**
1. Go to linkedin.com → Your profile → Recommendations received
2. Copy the recommendation text and the recommender's name/title
3. Add them to the `testimonials` array in `Testimonials.astro`

**Acceptance criteria:**
- Minimum 2 real testimonials displayed (ideally 3: manager, peer, client/stakeholder)
- Each card shows: avatar initials, name, title + company, quote text, "Via LinkedIn" tag
- LinkedIn CTA link remains at the bottom

---

### #007 · 🟡 Enhancement — Add Microsoft Learn transcript link to Certifications section
**Labels:** `enhancement` `content`
**File:** `src/components/Certifications.astro`

**Description:**
The Certifications section links to Credly for badge verification but not to
the Microsoft Learn transcript which shows all 23 passed exams, 15 active certs,
and 31 completed learning paths — a more comprehensive credential record.

**Acceptance criteria:**
- Add a second verification link: "View Microsoft Learn Transcript →"
- URL: `https://learn.microsoft.com/en-us/users/logan-dinesh/transcript`
- Place alongside the existing Credly link in the section header

---

### #008 · 🟡 Bug — `aria-expanded` missing on mobile hamburger button
**Labels:** `bug` `a11y`
**File:** `src/components/Nav.astro`

**Description:**
The mobile menu button `#menu-btn` has `aria-label="Toggle menu"` but no
`aria-expanded` attribute. Screen readers cannot announce whether the menu
is open or closed. The `aria-expanded` value must be toggled in JavaScript
alongside `classList.toggle('hidden')`.

**Fix:**
```js
btn.addEventListener('click', () => {
  const isOpen = !menu.classList.toggle('hidden');
  btn.setAttribute('aria-expanded', String(isOpen));
});
```

**Acceptance criteria:**
- `aria-expanded="false"` on page load
- Toggles to `aria-expanded="true"` when menu opens
- Passes axe or WAVE accessibility audit

---

### #009 · 🟡 Enhancement — Add `prefers-reduced-motion` support
**Labels:** `enhancement` `a11y` `ux`
**File:** `src/layouts/Layout.astro`, `src/components/Hero.astro`

**Description:**
The Hero section uses `animate-bounce` (scroll indicator) and `animate-pulse`
(status badge dot). Users with vestibular disorders or who have enabled
"Reduce Motion" in their OS settings will see unwanted animations.

**Fix:**
Add to `Layout.astro` global CSS or `tailwind.config.mjs`:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: 0.01ms !important; }
}
```
Or use Tailwind's `motion-reduce:` variant on each animated element.

**Acceptance criteria:**
- No animations play when OS reduced-motion preference is active
- Verified in macOS: System Settings → Accessibility → Reduce Motion

---

### #010 · 🟡 Enhancement — Upgrade Astro from 4.15 → 4.16+
**Labels:** `enhancement` `performance`
**File:** `package.json`

**Description:**
The project declares `"astro": "^4.15.0"` but Astro 4.16.19 (and 6.x)
are available. Running `npm install` already installs 4.16.x due to the `^`
range. The `package.json` version floor should be updated to reflect actual
installed version for CI/CD reproducibility.

**Acceptance criteria:**
- `package.json` reflects `"astro": "^4.16.0"` (or latest 4.x)
- `npm ci` in CI uses the locked version from `package-lock.json`
- No breaking changes (test with `npm run build`)

---

### #011 · 🟡 Enhancement — Add `<meta name="theme-color">` for mobile browser chrome
**Labels:** `enhancement` `ux`
**File:** `src/layouts/Layout.astro`

**Description:**
Mobile browsers (Chrome on Android, Safari on iOS) display a coloured strip
in the browser chrome that matches `theme-color`. Without it, the browser
chrome shows a default grey/white strip that clashes with the dark navy
portfolio aesthetic.

**Fix:**
```html
<meta name="theme-color" content="#0a0f1e" media="(prefers-color-scheme: dark)" />
<meta name="theme-color" content="#f9fafb" media="(prefers-color-scheme: light)" />
```

**Acceptance criteria:**
- Mobile browser chrome matches the site's active theme colour
- Tested on Chrome Android and Safari iOS

---

### #012 · 🟢 Enhancement — Add `favicon.ico` alongside `favicon.svg`
**Labels:** `enhancement` `ux`
**File:** `public/`

**Description:**
`favicon.svg` is modern and widely supported, but older browsers (IE, some
email clients) and certain desktop OS bookmarks require a `.ico` file.
Adding a 32×32 `favicon.ico` improves compatibility at no UX cost.

**Acceptance criteria:**
- `public/favicon.ico` present (32×32, multi-size ICO)
- `<link rel="icon" href="/favicon.ico" sizes="32x32">` added to `Layout.astro`
- Existing SVG favicon link retained for modern browsers

---

### #013 · 🟢 Enhancement — Add `apple-touch-icon` for iOS home screen
**Labels:** `enhancement` `ux`
**File:** `public/`, `src/layouts/Layout.astro`

**Description:**
When a user adds the portfolio to their iPhone/iPad home screen, iOS uses the
`apple-touch-icon` if defined. Without it, iOS captures a screenshot of the
page which looks poor as an icon.

**Fix:**
- Create `public/apple-touch-icon.png` (180×180px, no rounded corners — iOS applies them)
- Add `<link rel="apple-touch-icon" href="/apple-touch-icon.png" />` to `Layout.astro`

---

### #014 · 🟢 Enhancement — Pin `@astrojs/sitemap` to exact version in `package.json`
**Labels:** `enhancement`
**File:** `package.json`

**Description:**
`@astrojs/sitemap` was downgraded from 3.7.2 to 3.2.1 due to a compatibility
issue with Astro 4.x. The version in `package.json` should reflect this as an
exact pin (`3.2.1` not `^3.2.1`) to prevent a future `npm install` from
auto-upgrading to a broken version.

**Current:** `"@astrojs/sitemap": "^3.2.1"`
**Expected:** `"@astrojs/sitemap": "3.2.1"`

---

### #015 · 🟢 Enhancement — Self-host Google Fonts for privacy and performance
**Labels:** `enhancement` `performance`
**File:** `src/layouts/Layout.astro`

**Description:**
The site currently loads Inter and JetBrains Mono from `fonts.googleapis.com`.
This creates a third-party network dependency on every page load and sends
visitor IP data to Google (a GDPR concern for a German-hosted site).

**Fix options:**
1. Use [Fontsource](https://fontsource.org) npm packages:
   ```bash
   npm install @fontsource/inter @fontsource/jetbrains-mono
   ```
   Import in `Layout.astro`
2. Download fonts and serve from `public/fonts/`

**Acceptance criteria:**
- No requests to `fonts.googleapis.com` or `fonts.gstatic.com`
- Font load performance equal or better (no FOUT)
- Privacy policy compliant

---

## Resolved Issues (done in initial build)

| # | Issue | Resolution |
|---|---|---|
| ✅ | Compiler error from `<\!--` escaped HTML comments | Removed all escaped comments via Node.js script |
| ✅ | Copilot cert code listed as `MB-900` | Corrected to `AB-900` |
| ✅ | No sitemap generated | Added `@astrojs/sitemap` integration |
| ✅ | No OG tags / missing canonical | Added `og:*`, `twitter:*`, canonical to Layout |
| ✅ | No scroll-spy nav highlighting | Added IntersectionObserver in Nav script |
| ✅ | No light/dark mode | Implemented Tailwind `darkMode: 'class'` + toggle |
| ✅ | No profile avatar | Added DL gradient initials avatar in Hero + About |
| ✅ | No resume download | PDF in `public/`, download buttons in Nav/Hero/Contact/Footer |
| ✅ | Degree grade (2:2) shown publicly | Removed from Education section |
| ✅ | Phone number missing from Contact | Added `tel:+4915119496953` |
| ✅ | About info cards not clickable | Email and LinkedIn are now `<a>` elements |
| ✅ | No 404 page | Created branded `src/pages/404.astro` |
| ✅ | 4 projects missing (Purview, Proxmox, Exchange, Zero Trust) | Added to Projects section |
| ✅ | ISC2 CC cert missing | Added to active certifications grid |
| ✅ | `MyQualifications/` in repo | Added to `.gitignore` |

---

*Last updated: 2026-04-20 · Repository: `dineshlog/dineshlog.github.io`*
