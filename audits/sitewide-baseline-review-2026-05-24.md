# Sitewide Baseline Review — 2026-05-24

## Scope
- Reviewed repository structure and content files on May 24, 2026 (UTC).
- Reviewed all 23 HTML files and shared `css/styles.css`.
- Documentation-only audit update; no production code edits in this change.

## Audit Verification Status

### Manually verified in repository
- ✅ Route inventory and page count (23 HTML pages) verified.
- ✅ Global metadata coverage (`title`, `meta description`, canonical, OG, Twitter) verified across major templates.
- ✅ Conversion flow pattern and contact-entry points verified by reading nav and CTA links.
- ✅ Accessibility observations (form labeling, focus styling patterns) verified in source.
- ✅ Responsive/layout risk indicators verified in CSS media-query and grid rules.
- ✅ Asset reference integrity spot-check verified for market pages and OG image references.

### Not yet verified (outside this repo audit pass)
- ⏳ Runtime behavior in browser (actual keyboard tab order, focus visibility rendering, CLS/LCP measurements).
- ⏳ Server behavior (whether `/locations/*` routes are truly 301 redirected at hosting layer).
- ⏳ Analytics/tracking behavior for CTA and form submissions.

---

## Findings

### 1) Homepage lead form has no defined submission endpoint
- **Severity:** P1
- **File path(s):** `index.html`
- **Issue:** The contact form is present, but no `action` or `method` attribute is defined on `<form class="contact-form">`.
- **Why it matters:** Lead submissions may not persist anywhere, causing direct conversion loss.
- **Recommended fix:** Add explicit `method` and `action` (or JS handler plus backend endpoint), and add success/error UX with submission logging.

### 2) Sitewide conversion path depends on homepage anchor instead of local forms
- **Severity:** P2
- **File path(s):** `services/index.html`, `services/mobile-vehicle-patrols.html`, `services/on-foot-patrols.html`, `services/on-call-response.html`, `industries/index.html`, `industries/apartments.html`, `industries/construction.html`, `industries/hospitality.html`, `industries/retail.html`, `industries/hoa-community.html`, `markets/index.html`, `markets/charlotte/index.html`, `markets/charlotte/mobile-security-patrols/index.html`, `markets/charlotte/on-foot-patrols/index.html`, `markets/charlotte/on-call-response/index.html`, `markets/charlotte/apartment-security-patrols/index.html`, `markets/charlotte/construction-site-security/index.html`, `markets/charlotte/retail-security-patrols/index.html`, `markets/charlotte/hoa-community-patrols/index.html`, `why-watchmen.html`
- **Issue:** Many deep pages route “Request Assessment”/contact to `../index.html#contact` or similar homepage anchor links rather than capturing intent in-page.
- **Why it matters:** Adds friction for users landing on deep pages from search and can lower conversion rate.
- **Recommended fix:** Add reusable page-level conversion blocks (short form or tracked CTA module) on service, industry, and market detail pages.

### 3) Missing JSON-LD on 10 pages creates structured-data inconsistency
- **Severity:** P2
- **File path(s):** `industries/index.html`, `industries/apartments.html`, `industries/construction.html`, `industries/hospitality.html`, `industries/retail.html`, `industries/hoa-community.html`, `markets/index.html`, `markets/charlotte/index.html`, `locations/index.html`, `locations/charlotte/index.html`
- **Issue:** These pages do not include `application/ld+json`, while other pages do.
- **Why it matters:** Uneven structured-data coverage weakens entity/page-type signaling across the information architecture.
- **Recommended fix:** Add a consistent JSON-LD baseline for hubs and legacy-route pages (Organization + WebPage + BreadcrumbList where relevant).

### 4) Legacy `/locations/` pages remain crawlable HTML instead of redirect-only routes
- **Severity:** P2
- **File path(s):** `locations/index.html`, `locations/charlotte/index.html`
- **Issue:** Legacy location pages include moved-content messaging and canonicals but are still full HTML pages in repo.
- **Why it matters:** Legacy URLs may continue to receive traffic and split canonical authority if server redirects are not enforced.
- **Recommended fix:** Implement server-level 301 redirects from `/locations/*` to corresponding `/markets/*` routes and keep canonicals aligned.

### 5) Broken/missing asset references on multiple Charlotte market pages
- **Severity:** P1
- **File path(s):** `markets/charlotte/apartment-security-patrols/index.html`, `markets/charlotte/hoa-community-patrols/index.html`, `markets/charlotte/retail-security-patrols/index.html`, `assets/charlotte-construction-security-patrol-suv.jpg`, `assets/watchmen-patrol-unit-multifamily.jpg`
- **Issue:** Pages reference `assets/charlotte-security-team.jpg` for inline image + OG/Twitter image, but this file is not present in `assets/`.
- **Why it matters:** Causes broken on-page images and social-preview image failures on affected pages.
- **Recommended fix:** Add the missing asset or update image references to an existing image file across HTML and social meta tags.

### 6) Contact form accessibility: no visible persistent labels
- **Severity:** P2
- **File path(s):** `index.html`
- **Issue:** Inputs rely on placeholders and `aria-label` attributes without visible `<label>` elements.
- **Why it matters:** Placeholder-only labeling reduces usability and accessibility clarity, especially after typing begins.
- **Recommended fix:** Add visible `<label for="...">` for each input/select/textarea and retain ARIA only where supplemental.

### 7) Focus-state coverage is inconsistent across interactive components
- **Severity:** P2
- **File path(s):** `css/styles.css`
- **Issue:** Styles heavily define `:hover`; only some components explicitly include `:focus-visible` (e.g., `.card-link`), while global link/button focus treatment is inconsistent.
- **Why it matters:** Keyboard users can lose clear focus indication in dark-theme UI, impacting accessibility and task completion.
- **Recommended fix:** Add consistent, high-contrast `:focus-visible` styles for all anchors, buttons, and form controls.

### 8) CSS file contains repeated section blocks, increasing maintenance overhead
- **Severity:** P2
- **File path(s):** `css/styles.css`
- **Issue:** Similar nav/hero/grid/CTA style blocks are repeated multiple times for different page families inside one stylesheet.
- **Why it matters:** Duplicated declarations increase drift risk and make updates/error fixes slower and less predictable.
- **Recommended fix:** Refactor into shared base component classes plus small page-specific modifiers; remove duplicated blocks.

### 9) Responsive risk: navigation and CTA layouts rely on multiple breakpoint-specific grid rewrites
- **Severity:** P3
- **File path(s):** `css/styles.css`
- **Issue:** Navigation and CTA grids are redefined repeatedly across several breakpoint groups (`700px`, `860px`, etc.), including overlapping rules.
- **Why it matters:** Overlapping breakpoint logic raises regression risk and can produce inconsistent rendering between page templates.
- **Recommended fix:** Consolidate breakpoint strategy and reuse one responsive nav/CTA pattern across templates.

### 10) Metadata quality is broadly present, but social images are not uniformly reliable
- **Severity:** P2
- **File path(s):** `index.html`, `services/index.html`, `services/mobile-vehicle-patrols.html`, `industries/index.html`, `industries/apartments.html`, `markets/index.html`, `markets/charlotte/index.html`, `markets/charlotte/retail-security-patrols/index.html`, `why-watchmen.html`
- **Issue:** Pages generally include OG/Twitter tags, but referenced image reliability is inconsistent (notably missing `charlotte-security-team.jpg` on some market pages).
- **Why it matters:** Social share previews may degrade for specific pages, weakening click-through and trust.
- **Recommended fix:** Run a repo-wide social-image integrity check and enforce an image-exists validation in content QA.

### 11) Site taxonomy split is visible in active structure and internal references
- **Severity:** P2
- **File path(s):** `markets/index.html`, `markets/charlotte/index.html`, `locations/index.html`, `locations/charlotte/index.html`, `index.html`
- **Issue:** Active IA emphasizes `/markets/` while legacy `/locations/` pages still ship in repository, indicating mixed route model.
- **Why it matters:** Mixed taxonomy can confuse users and content operators and complicates long-term SEO governance.
- **Recommended fix:** Complete route migration plan with redirects, remove legacy route pages once redirects are stable, and standardize documentation on `/markets/`.

### 12) Quantitative trust claims lack timestamp/source qualifiers
- **Severity:** P3
- **File path(s):** `index.html`
- **Issue:** Homepage includes quantitative claim language (e.g., “180+ Current Weekly Patrols”) without source date context.
- **Why it matters:** Operational claims can age quickly and reduce credibility if not maintained.
- **Recommended fix:** Add “as of” date qualifiers and include periodic review ownership in content governance docs.

---

## Audit Status Notes (replacing checklist placeholders)
- **Status:** Baseline documentation now includes repository-grounded findings with concrete paths and observations.
- **Verified now:** Route/page inventory, nav/link patterns, SEO/meta presence patterns, JSON-LD distribution, asset-reference integrity spot checks, and CSS maintainability/responsive risk indicators.
- **Pending later verification:** Browser-rendered accessibility behavior, production redirect behavior, and analytics instrumentation outcomes.
