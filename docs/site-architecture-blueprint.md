# Watchmen Solutions Website Architecture Blueprint

## Purpose & Scope
This blueprint defines a scalable information architecture for evolving the current Charlotte metro patrol campaign site into a full Watchmen Solutions brand website aligned with the Security Athlete® standard. It establishes governance for SEO, navigation, page intent, internal linking, canonicalization, and market expansion.

---

## 1) Full Sitemap Architecture

### 1.1 Top-Level Sitemap (Future Mature State)

```text
/
├── /about/
├── /services/
│   ├── /services/mobile-patrol/
│   ├── /services/alarm-response/
│   ├── /services/fire-watch/
│   ├── /services/construction-site-security/
│   └── /services/[additional-global-service]/
├── /industries/
│   ├── /industries/apartment-complexes/
│   ├── /industries/hoa-communities/
│   ├── /industries/retail-centers/
│   ├── /industries/industrial-properties/
│   └── /industries/[additional-global-industry]/
├── /markets/
│   ├── /markets/charlotte-nc/
│   │   ├── /markets/charlotte-nc/services/
│   │   │   ├── /markets/charlotte-nc/services/mobile-patrol/
│   │   │   ├── /markets/charlotte-nc/services/alarm-response/
│   │   │   └── /markets/charlotte-nc/services/[service]/
│   │   ├── /markets/charlotte-nc/industries/
│   │   │   ├── /markets/charlotte-nc/industries/apartment-complexes/
│   │   │   ├── /markets/charlotte-nc/industries/hoa-communities/
│   │   │   └── /markets/charlotte-nc/industries/[industry]/
│   │   ├── /markets/charlotte-nc/areas-served/
│   │   └── /markets/charlotte-nc/contact/
│   └── /markets/[future-market-slug]/
├── /resources/
│   ├── /resources/blog/
│   ├── /resources/guides/
│   ├── /resources/case-studies/
│   └── /resources/[article-slug]/
├── /contact/
├── /request-quote/
├── /assessment/
└── /privacy-policy/ + /terms/
```

### 1.2 Architecture Layers

| Layer | Role | Typical URL Pattern |
|---|---|---|
| Root brand layer | Brand positioning, trust, conversion routing | `/`, `/about/`, `/contact/` |
| Markets hub | Entry point to all geo markets | `/markets/` |
| Metro market hubs | Local authority + conversion | `/markets/{market-slug}/` |
| Market service pages | Geo+service intent | `/markets/{market-slug}/services/{service-slug}/` |
| Market industry pages | Geo+industry intent | `/markets/{market-slug}/industries/{industry-slug}/` |
| Global service pages | Non-geo service authority | `/services/{service-slug}/` |
| Global industry pages | Non-geo industry authority | `/industries/{industry-slug}/` |
| Blog/resource layer | Topical education + internal-link support | `/resources/...` |
| Contact/conversion pages | Lead capture | `/contact/`, `/request-quote/`, `/assessment/` |

---

## 2) URL Conventions: `/markets/` vs `/locations/`

| Option | Pros | Cons |
|---|---|---|
| `/markets/` | Better reflects strategic expansion units; aligns with “metro market” language; supports broader business segmentation if needed | Slightly less familiar to some local-search users |
| `/locations/` | Familiar to users searching for “locations” | Implies office addresses more than service areas; less strategic for metro ecosystems |

**Primary recommendation: `/markets/`**

**Why:**
1. It matches the long-term operating model (metro market ecosystems, not storefront directories).
2. It allows future scaling where a market may include multiple municipalities/suburbs.
3. It semantically separates *where you operate strategically* from simple office-location listings.

**Rule:** Use `/markets/` as canonical structure. If `/locations/` is ever used for legacy access, 301 redirect it to `/markets/` equivalents.

---

## 3) Naming Standards

| Element | Standard | Example |
|---|---|---|
| Page Title | `Primary Topic | Market (if local) | Watchmen Solutions` | `Mobile Patrol Security | Charlotte, NC | Watchmen Solutions` |
| H1 | Human-readable intent phrase, no pipes | `Mobile Patrol Security in Charlotte, NC` |
| Meta Description | 140–160 chars, include service/industry + market where relevant + CTA | `Need mobile patrol in Charlotte, NC? Watchmen Solutions delivers Security Athlete® standards. Request a quote today.` |
| Filenames/Folders | lowercase, hyphenated, stable slugs, trailing slash URLs | `/markets/charlotte-nc/services/mobile-patrol/` |
| Nav Labels | Short, plain language | `Services`, `Industries`, `Markets`, `Resources`, `Contact` |
| CTA Labels | Action + outcome clarity | `Request a Quote`, `Book a Security Assessment`, `Talk to an Expert` |

### Additional Rules
- No year/version in slugs unless content is explicitly time-bound.
- Keep slugs consistent across global and local layers (`mobile-patrol` everywhere).
- Market slug format: `{city}-{state-abbrev}` (e.g., `charlotte-nc`).

---

## 4) Breadcrumb Standards

| Page Type | Breadcrumb Pattern | Example |
|---|---|---|
| Global service/industry pages | `Home > Section > Page` | `Home > Services > Mobile Patrol Security` |
| Market hub pages | `Home > Markets > Market` | `Home > Markets > Charlotte, NC` |
| Market service pages | `Home > Markets > Market > Services > Service` | `Home > Markets > Charlotte, NC > Services > Mobile Patrol Security` |
| Market industry pages | `Home > Markets > Market > Industries > Industry` | `Home > Markets > Charlotte, NC > Industries > Apartment Complex Security` |
| Blog/resource posts | `Home > Resources > Type > Post` | `Home > Resources > Blog > How to Deter Parking Lot Crime` |

**Implementation rule:** Use visible breadcrumbs + matching schema markup (`BreadcrumbList`) with exact URL hierarchy.

---

## 5) Navigation Hierarchy

### 5.1 Current Campaign State (Near-Term)
**Desktop primary nav:**
- Home
- Services
- Industries
- Markets (single-item dropdown with Charlotte)
- Resources (or hidden until launched)
- Contact

**Mobile nav:** same order, accordion for Markets.

### 5.2 Mature State
**Desktop primary nav:**
- Services (mega or dropdown)
- Industries (dropdown)
- Markets (dropdown + “View All Markets”)
- Resources
- About
- Contact
- Persistent CTA button: `Request a Quote`

**Markets dropdown behavior:**
- First item: `View All Markets` → `/markets/`
- Then top markets by business priority (e.g., Charlotte, Raleigh, Nashville...)
- If long list grows, group alphabetically or by region.

### 5.3 Primary Nav vs Footer

| Put in Primary Nav | Put in Footer |
|---|---|
| High-intent conversion paths | Utility/legal pages |
| Services, Industries, Markets | Privacy, Terms, Sitemap |
| Contact and quote CTA | Full market list (if too long for header) |
| Resources | Certifications, compliance, social links |

---

## 6) Internal Linking Rules

### Core Link Flow
1. **Homepage** links to: top global services, top industries, markets hub, Charlotte hub, quote/contact.
2. **Markets hub** links to every market hub + key global service/industry anchors.
3. **Charlotte hub** links to all Charlotte services/industries + global counterparts.
4. **Global service pages** link to:
   - related global industries,
   - markets hub,
   - top local service variants (starting with Charlotte).
5. **Market service pages** link to:
   - parent market hub,
   - matching global service page,
   - related local industry pages,
   - conversion pages.
6. **Global industry pages** link to relevant global services + markets hub.
7. **Market industry pages** link to matching global industry + local services + market hub.
8. **Blog/resource articles** must link upward (global + local commercial pages) and laterally (related resources).

### Anchor Text Rules
- Use descriptive anchors (`mobile patrol security in Charlotte`) not generic (`click here`).
- Rotate natural variants to avoid over-optimization.
- Ensure every market service page has at least 3 internal inlinks from non-navigation content.

---

## 7) Page Intent Definitions

| Page Type | Primary SEO Intent | Conversion Intent | Content Requirements | Avoid | Cannibalization Guardrail |
|---|---|---|---|---|---|
| Root homepage | Brand + broad category terms | Route user to best next step | UVP, trust signals, service summary, market pathway | Deep local copy blocks | Do not target city-specific primary keyword in H1/title |
| Markets hub | “security services in [state/region]” discovery | Push to market hubs | Market cards, area context, links to markets | Duplicate service copy | No long-form service targeting here |
| Market hub | “[city] security company” | Local quote/contact | Local proof, local service list, area served | Global-only generic copy | Reserve city-head term for market hub only |
| Global service | “what/why service” non-geo | Cross-market service inquiry | Service explainer, FAQs, industries served | Heavy city repetition | Do not optimize for city+service as primary |
| Market service | “[service] in [city]” | Service-specific local leads | Local pain points, process, testimonials, SLA/coverage | Reused global copy blocks | One primary city+service target per URL |
| Global industry | “security for [industry]” | Industry consultations | Industry risks, solutions, compliance concerns | Geo stuffed variants | No city modifiers in core headings |
| Market industry | “[industry] security in [city]” | Industry-local leads | Localized scenarios, applicable services | Thin localization | Distinct from market service via industry framing |
| Resource/blog | Informational long-tail | Assisted conversion | Educational depth, examples, internal links | Hard sales-only copy | Do not target exact money-term keyword as primary |
| Contact/quote | Brand + transactional | Form completion/calls | Friction-light form, trust assurances | SEO bloat text | Keep no competing informational focus |

---

## 8) Canonical Strategy

### Canonical Rules
- **Self-canonical** on all indexable pages.
- **Global vs Market pages:** never canonical market pages to global pages (or vice versa) when intent differs (geo vs non-geo).
- **Index/noindex:**
  - Index: homepage, markets hub, market hubs, core services/industries, resource content with quality threshold.
  - Noindex: thin tag-like archives, duplicate filtered pages, internal utility pages.

### Duplicate-Content Avoidance
- Require meaningful local deltas on market pages (proof, geography, case examples, response specifics).
- Reuse structure, not copy.
- Maintain unique title/H1/meta for each page.

### GitHub Pages vs Production Domain
- During GitHub Pages staging, canonicals should point to the staging URL **only if production URL is not live**.
- On production launch day, switch canonicals to final production domain in one release.
- Enforce 301 redirect mapping from GitHub Pages path patterns to production URL equivalents where possible.

---

## 9) Future Expansion Governance (New Market Launch Rules)

### 9.1 Required Pages Before Any Market Goes Live
Minimum required set:
1. Market hub page
2. At least 3 market service pages (priority services)
3. At least 2 market industry pages (priority verticals)
4. Market contact/conversion endpoint
5. Area-served subsection or equivalent coverage explanation

### 9.2 Minimum Unique Local Content Requirements
- Market hub: minimum 40% unique narrative copy.
- Market service/industry pages: minimum 30% unique local copy each.
- At least 2 market-specific proof elements per page (local testimonial, landmark coverage, response model, local regulations context, etc.).

### 9.3 Linking Checklist
- Included in `/markets/` hub cards.
- Included in markets dropdown (or staged once market is public).
- Bi-directional links between market hub and all child pages.
- Links from global service/industry pages to new market equivalents where relevant.

### 9.4 SEO Metadata Checklist
- Unique title, H1, meta description.
- Self-canonical set.
- OpenGraph/Twitter tags.
- Schema where applicable (LocalBusiness for market hubs, BreadcrumbList sitewide).
- XML sitemap inclusion.

### 9.5 QA Checklist
- All CTAs route correctly.
- Mobile nav and dropdown behavior verified.
- No broken links.
- Core Web Vitals sanity check.
- Indexability rules verified (robots/canonical/noindex).
- Analytics/conversion events firing.

### 9.6 Branch/PR Workflow Recommendation
- Branch pattern: `feature/market-{slug}-launch`.
- PR template includes: page inventory, metadata checklist, internal links checklist, screenshots, QA evidence.
- Require at least one reviewer for SEO architecture and one for content quality.
- Launch with redirect map and sitemap update in same deployment.

---

## 10) Charlotte-Specific Near-Term Evolution Plan

### Objective
Preserve Charlotte campaign performance while transitioning root `/` to a durable brand homepage.

### Recommended Path
1. **Phase 1:** Keep Charlotte assets live; introduce `/markets/charlotte-nc/` as the canonical Charlotte hub structure.
2. **Phase 2:** Reposition root homepage to brand + Security Athlete® framework with clear paths to Charlotte and core services.
3. **Phase 3:** Add first global service and industry hub pages; ensure Charlotte pages link to/from them.
4. **Phase 4:** Expand resource layer and build topical support content feeding Charlotte + global pages.
5. **Phase 5:** Launch second market using governance checklist; validate repeatable system.

### Conflict-Avoidance Rules
- Root homepage should not compete for Charlotte-specific primary query terms.
- Charlotte market hub owns city-head terms; Charlotte service/industry pages own geo-modified long-tail terms.
- Maintain consistent IA so future markets can clone structure without URL exceptions.

---

## Recommended Implementation Phases

| Phase | Focus | Deliverable |
|---|---|---|
| 0 | Planning | Final IA + naming + canonical standards |
| 1 | Foundation | Root brand homepage + markets hub + Charlotte mapped under `/markets/` |
| 2 | Global Layer | Core global services + industries |
| 3 | Local Depth | Charlotte service/industry expansion and interlink optimization |
| 4 | Content Engine | Resources/blog framework + editorial linking model |
| 5 | Scale | Launch additional markets with governance controls |

---

## Risks & Guardrails

| Risk | Impact | Guardrail |
|---|---|---|
| Root vs Charlotte keyword overlap | Ranking cannibalization | Enforce intent map and ownership matrix |
| Thin local clones for new markets | Low trust + poor rankings | Unique-content minimum thresholds + proof requirements |
| Overcrowded navigation as markets grow | UX friction | “View All Markets” hub + footer full list |
| Canonical misconfiguration during migration | Deindexing/duplication | Release-day canonical/redirect checklist |
| Blog content not tied to commercial pages | Low conversion value | Mandatory internal-link targets per article |

---

## Architecture Decision Summary
- Standardize on `/markets/` as the location framework.
- Build a dual-layer SEO model: global authority pages + localized market pages.
- Keep Charlotte as the first mature market model under the same repeatable taxonomy.
- Use strict governance for metadata, canonicalization, and internal linking to scale safely.
