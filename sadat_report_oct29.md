# Daily Work & Progress Report — Insights & About Sections

**Date:** 29 October 2026  
**Developer:** Tahmid Sadat (`TahmidSadat02`)  
**Designation:** Software Engineer / Full-Stack & CMS Developer  
**Branch:** `sadat` (Tracking `origin/wary`, synced with `origin/production`)  
**Target Database:** Remote PostgreSQL (`postgres://simal_cms_user:***@182.160.112.66:5434/simal-cms`)  
**Active Services:**
- Public Site: Astro SSG / SSR (`http://127.0.0.1:8000/`)
- CMS Portal: Node.js / Express Admin Studio (`http://127.0.0.1:8090/cms.html`)  
**Status:** Completed, Verified & Persisted in Remote Database  

---

## 📌 Executive Summary

Today's work focused on comprehensive structural upgrades, database-driven content synchronization, Zero-JSON visual inspector implementation, and end-to-end editorial verification for two critical sections of the platform:
1. **The "About" Section & Company Page (`/about/` and `/ar/about/`)**:
   - Upgraded Section Headings and Operational Principles for **Core Values** (`values`) and **Executive Leadership** (`leadership`) across both English and Arabic with dedicated direct SQL migrations.
   - Built custom Zero-JSON visual matrix-array repeaters in CMS Studio (`cms.js`) enabling content editors to add, modify, and delete principles and leadership rows with instant database persistence.
   - Fixed topbar navigation overlap on section anchor jumps by adding `scroll-margin-top` to `#leadership` and related section anchors.
   - Extended and verified the **Corporate Legal Entities** (`entities`) studio with live public website rendering previews, dynamic asset proxying, and auto-save for entity avatar graphics.
   - Verified and fine-tuned About Hero (`about`), Metric Counters (`about-counters`), Corporate Milestones timeline (`milestones`), Awards & Testimonials (`about-awards`), and Corporate Social Responsibility (`csr`).

2. **The "Insights" Hub & Article Desk (`/insights/` and `/ar/insights/`)**:
   - Modernized the **Insights Hero & Monthly Digest Studio** (`insights-hero`) with human-friendly visual controls for badges, H1 headlines, lead statements, and the newsletter card subscription controls.
   - Enhanced the **Gated Line Card Download Module** (`insights-linecard`) with structured visual inputs for card icons, titles, release notes, and gated CTA parameters.
   - Audited and updated **13 individual Insight Articles** (`insight:*`) through the dedicated CMS Article Inspector with zero raw JSON exposure, ensuring bilingual parity and live website preview accuracy.
   - Verified dynamic direct SQL parameterization in Astro frontmatter for both English and Arabic RTL routes.

---

## 1. 🏢 "About" Section Architecture & Upgrades

### 1.1 Section Eyebrows & Main Headings Integration (`values` & `leadership`)
- **Problem Diagnosed:**
  - Previously, the `values` and `leadership` content blocks stored only flat arrays of items without dedicated fields for the section eyebrow and main heading.
  - Headings were either hardcoded in Astro templates or could not be customized by content administrators via the CMS.
- **Direct Remote SQL Migration (`database/sql/50_add_values_and_leadership_headings.sql`):**
  - Authored an idempotent SQL migration restructuring `content_blocks` for `entity='site'` and `entity_id IN ('values', 'leadership')` for both English (`en`) and Arabic (`ar`).
  - Transformed payload structure from flat arrays to structured JSON objects containing `eyebrow`, `heading`, and `items`:
    - **`values` (EN):** Eyebrow: *"Mission, vision & values"* | Heading: *"Why Simal exists"* | 4 Operational Principle pairs.
    - **`values` (AR):** Eyebrow: *"قيمنا الراسخة"* | Heading: *"المبادئ التوجيهية للعمل"* | 5 Authentic Arabic Principles.
    - **`leadership` (EN):** Eyebrow: *"Board of directors"* | Heading: *"Leadership team"* | 3 Executive Board members.
    - **`leadership` (AR):** Eyebrow: *"القيادة التنفيذية"* | Heading: *"فريق الإدارة ومجلس الإدارة"* | 3 Executive Board members.
- **Zero-JSON Visual Inspectors in CMS Studio (`apps/portal/public/assets/js/cms.js`):**
  - **`renderValuesInspector(p, id)`**:
    - Provides clean single-line text inputs for Section Eyebrow and Main Section Heading.
    - Built a structured matrix-array repeater (`data-vfield-type="matrix-array"`) with `＋ Add Row` and `✕ Delete` buttons for Principle Titles and Descriptions.
  - **`renderLeadershipInspector(p, id)`**:
    - Section header controls for Leadership Eyebrow and Leadership Heading.
    - Interactive matrix repeater for Executive Role / Title and Full Name.
    - Fixed matrix row deletion handler syntax (`c858951`) and wired automatic real-time state serialization on save.
- **Astro Frontend Dynamic Rendering:**
  - Updated `apps/site/src/pages/about/index.astro` and `apps/site/src/pages/ar/about/index.astro`.
  - Replaced static markup with dynamic parameterized queries:
    ```typescript
    const leadershipRaw = await block('leadership', defaultLeadership);
    const leadershipEyebrow = leadershipRaw?.eyebrow || 'Board of directors';
    const leadershipHeading = leadershipRaw?.heading || 'Leadership team';
    const leadership = Array.isArray(leadershipRaw?.items) ? leadershipRaw.items : [];
    ```
  - Preserved 100% of existing CSS classes, grid structures, and typography tokens.

---

### 1.2 Sticky Header Anchor Offset Fix (`dab129e`)
- **Issue:** Clicking navigation anchor links (e.g. `#leadership`, `#values`, `#milestones`) caused the browser to scroll section headings directly underneath the fixed top header bar, partially obscuring the title copy.
- **Remediation:** Added `scroll-margin-top: 100px;` (and responsive offsets) to section anchor targets in both `apps/site/src/pages/about/index.astro` and `apps/site/src/pages/ar/about/index.astro`.
- **Result:** Smooth anchor navigation scrolls cleanly with comfortable breathing room below the sticky header.

---

### 1.3 Corporate Legal Entities Inspector & Avatar Dynamic Proxy (`a97d153`, `7981a13`)
- **Visual Entity Card Studio (`renderEntityCard`):**
  - Integrated a dedicated card repeater for `site:entities` managing all group companies (Simal Technologies Middle East LLC, StarSeed Technologies, TwinMOS Technologies, UniSoft Solutions).
  - Configurable attributes per entity: Entity Code, Display Name, Jurisdiction Badge, Brand Accent Color, Circular Avatar/Logo, Subtitle, and Border Accent.
  - Added an embedded **Live Website Rendering Preview Box** directly within the inspector so administrators can observe how each card renders on `/about/` before publishing.
- **Dynamic Asset Proxy & Auto-Save:**
  - Implemented dynamic avatar image proxying (`/api/cms/media/file/...`) with graceful fallback to company initial badges.
  - Added auto-save triggers ensuring entity updates synchronize to PostgreSQL without manual page reloads.

---

### 1.4 About Page Subsections Verified in Database
| Content Block ID | Lang | Sections Covered | Field Controls in CMS | DB Status |
| :--- | :---: | :--- | :--- | :---: |
| `site:about` | `en` / `ar` | Hero Banner & Brand Story | Heritage Badge, Main H1 Headline, Mission & Vision statements | Verified ✅ |
| `site:about-counters` | `en` / `ar` | KPI Metrics Strip | Years in channel (24+), Authorized brands (26+), Live SKUs, Regions | Verified ✅ |
| `site:entities` | `en` / `ar` | Corporate Legal Entities (4 cards) | Code, Name, Jurisdiction Badge, Logo/Avatar, Subtitle, Accent | 23 Saves ✅ |
| `site:milestones` | `en` / `ar` | Corporate Timeline (2002–2026) | Year, Title, Description, Highlight Toggle checkbox | 11 Saves ✅ |
| `site:values` | `en` / `ar` | Core Values & Principles Matrix | Eyebrow, Main Heading, 4–5 Principle rows (Title + Description) | Verified ✅ |
| `site:leadership` | `en` / `ar` | Board of Directors & Executives | Eyebrow, Main Heading, Executive rows (Role + Full Name) | Verified ✅ |
| `site:about-awards` | `en` / `ar` | Awards & Testimonials Strip | Section Header, Authorized Brands badge, Awards list, Customer Quotes | 5 Saves ✅ |
| `site:csr` | `en` / `ar` | Corporate Social Responsibility | Section Eyebrow, Main Heading, Program Body Text, Initiative Tags | 6 Saves ✅ |

---

## 2. 📰 "Insights" Hub & Articles Modernization

### 2.1 Insights Hero & Monthly Digest Newsletter Studio
- **Visual Inspector (`renderInsightsHeroInspector`):**
  - Implemented a 3-tier visual card inspector in `cms.js` managing `site:insights-hero`:
    1. **Insights Hub Header:** Hero Eyebrow Badge, Main H1 Headline, and Lead Statement textarea.
    2. **Monthly Digest Newsletter Card:** Digest Card Title (e.g. `📬 Monthly digest`), Description, Work Email Input Placeholder, and Subscribe Button Label.
    3. **Articles Section Header:** Articles Eyebrow (`Latest`) and Dynamic Count Heading (`From the desks — {count} articles, every one opens in full`).
- **Database & CMS Activity:**
  - 40 CMS save revisions recorded today in `cms_revisions`.
  - Tested dynamic `{count}` placeholder substitution: accurately interpolates the total count of active published articles fetched from the database.

---

### 2.2 Gated 2026 Line Card Download Module
- **Visual Inspector (`renderInsightsLinecardInspector`):**
  - Dedicated visual form managing `site:insights-linecard` without raw JSON:
    - Section Eyebrow (`Line card`)
    - Main Section Heading (`Download the 2026 line card`)
    - Card Icon Picker / Emoji input (`📄`)
    - Card Title (`Simal 2026 line card — all 21 brands, 78 SKUs`)
    - Card Subtitle / Note (`PDF · updated monthly · includes Peppol readiness checklist`)
    - Gated Email Placeholder (`Work email (gated)`)
    - Submit CTA Button Label (`Get the PDF →`)
- **Database Persistence:** 45 revisions executed and verified against PostgreSQL.

---

### 2.3 Insight Articles Editorial Management (`insight:*`)
- **Dedicated Article Inspector (`renderInsightArticleInspector`):**
  - Full visual management for individual insight publications:
    - Article Headline / Title
    - Editorial Category dropdown (`STORAGE`, `GUIDE`, `CHANNEL`, `COMPLIANCE`, `HARDWARE`, `SOFTWARE`, `AI`, `CLOUD`, `CYBERSECURITY`, `INDUSTRY`)
    - Author / Byline and Publication Date picker
    - Read Time number spinner (minutes)
    - Comma-separated Tag inputs
    - Call-To-Action (CTA) label and destination URL
    - Multi-section body content repeaters
- **13 Published Articles Audited & Updated Today:**
  1. `insight:authorized-it-distributors-uae:en` (14 revisions)
  2. `insight:unierp-transforms-sme:en` (7 revisions)
  3. `insight:why-gray-market-costs-more:en` (6 revisions)
  4. `insight:uae-data-protection-pdpl:en` (6 revisions)
  5. `insight:top-ssds-for-resellers-2025:en` (6 revisions)
  6. `insight:simal-two-decades:en` (6 revisions)
  7. `insight:industry-4-manufacturing-erp:en` (6 revisions)
  8. `insight:future-proof-it-infrastructure:en` (6 revisions)
  9. `insight:enterprise-ssd-buying-guide-2026:en` (6 revisions)
  10. `insight:ddr4-vs-ddr5:en` (6 revisions)
  11. `insight:cybersecurity-best-practices-uae:en` (6 revisions)
  12. `insight:crucial-p510-launch:en` (6 revisions)
  13. `insight:ai-in-erp-2026:en` (6 revisions)

---

### 2.4 Frontend Integration & Bilingual Arabic Parity
- **English Route (`apps/site/src/pages/insights/index.astro`):**
  - Dynamically fetches published articles, hero block, and line card block via direct SQL:
    ```typescript
    const rows = await db`SELECT entity_id, payload FROM content_blocks WHERE entity = 'insight' AND lang = 'en' ORDER BY payload->>'at' DESC NULLS LAST`;
    const [heroRow] = await db`SELECT payload FROM content_blocks WHERE entity = 'site' AND entity_id = 'insights-hero' AND lang = 'en'`;
    const [linecardRow] = await db`SELECT payload FROM content_blocks WHERE entity = 'site' AND entity_id = 'insights-linecard' AND lang = 'en'`;
    ```
  - Featured article hero card, category filter pills (`All`, `Storage`, `Guide`, `Channel`), and article grid render dynamically from DB.
- **Arabic Twin Route (`apps/site/src/pages/ar/insights/index.astro`):**
  - Seamless fallback via SQL `COALESCE(ar.payload, en.payload)` ensuring zero 404s or broken articles.
  - Native RTL direction (`dir="rtl"`), Arabic localized dates (`ar-EG`), and authentic Arabic copy for all headings, digest forms, and line card modules.

---

## 3. 🛠️ Supporting Fixes & CMS Stability Enhancements

In support of the About and Insights work, several core CMS stability improvements were committed:
1. **Highlight Checkbox Toggle Logic (`bafe8c6`):**
   - Automatically renders boolean attributes (e.g. `highlight`, `is_active`, `featured`) as native checkboxes with dynamic `Enabled / True` and `Disabled / False` status labels.
   - Ensures newly added repeater items initialize booleans properly without causing type mismatch bugs on save.
2. **Auto-Save on Repeater Item Deletion (`3903bd8`, `370d340`):**
   - Streamlined item deletion in repeaters to immediately serialize and persist changes to the database without requiring multi-step confirmation dialogs.
3. **Matrix Delete Handler Fix (`c858951`):**
   - Resolved a JavaScript syntax error in matrix repeater row removal and bumped the cache buster to `v=2026102912`.
4. **Content Cleanups (`adeaabf`, `66f77fd`, `4795445`):**
   - Cleaned up obsolete subsections (Culture/Workspace graphic from Careers, Media Assets/Brand Pack from Resources, and emoji selectors from Section 10 Why Choose Simal) via dedicated SQL scripts (`48_...sql`, `49_...sql`, `51_...sql`).

---

## 4. 📊 Git Commits Log (Tahmid Sadat — 29 Oct 2026)

| Commit Hash | Component | Summary of Changes |
| :---: | :---: | :--- |
| `ac90851` | `cms, site` | Add section eyebrow and main heading to values and leadership sections (`50_add_values_and_leadership_headings.sql`) |
| `dab129e` | `site` | Add `scroll-margin-top` to leadership and section anchors on about page to fix topbar overlap |
| `a97d153` | `cms` | Dedicated inspector with full RUD operations, live preview, and persistent save for Corporate Legal Entities |
| `7981a13` | `site, portal` | Dynamic asset proxy and auto-save for entity avatars |
| `3903bd8` | `cms` | Auto-save section on repeater item deletion and bump cache-buster |
| `bafe8c6` | `cms` | Render highlight option as checkbox for newly created items and add dynamic toggle logic |
| `370d340` | `cms` | Make delete option immediately persist without blocking confirm dialog and bump cache version |
| `c858951` | `cms` | Fix syntax error in matrix delete handler and bump cache buster to `v=2026102912` |
| `adeaabf` | `cms, db` | Remove emojis and icon picker from Section 10 Why Choose Simal and apply DB cleanup (`48_...sql`) |
| `66f77fd` | `cms, db` | Remove Culture / Workspace Graphic subsection from Careers page editor (`49_...sql`) |
| `4795445` | `cms, db` | Remove Downloadable Media Assets & Brand Pack section from resources page editor (`51_...sql`) |

---

## 5. 📋 Verification & Operational Sign-Off Matrix

| Section / Capability | Verification Method | Target URL / Endpoint | Status |
| :--- | :--- | :--- | :---: |
| **About Hero & Story** | CMS Visual Edit + Live Site Render | [`/about/`](http://127.0.0.1:8000/about/) & [CMS](http://127.0.0.1:8090/cms.html#/block-editor?entity=site&id=about&lang=en) | **Verified** ✅ |
| **Corporate Legal Entities** | Live Preview Box + RUD Operations | [`/about/`](http://127.0.0.1:8000/about/) & [CMS](http://127.0.0.1:8090/cms.html#/block-editor?entity=site&id=entities&lang=en) | **Verified (23 Saves)** ✅ |
| **Core Values Studio** | Matrix Repeater + Heading Inputs | [`/about/`](http://127.0.0.1:8000/about/) & [CMS](http://127.0.0.1:8090/cms.html#/block-editor?entity=site&id=values&lang=en) | **Verified** ✅ |
| **Leadership Studio** | Matrix Repeater + Executive Rows | [`/about/`](http://127.0.0.1:8000/about/) & [CMS](http://127.0.0.1:8090/cms.html#/block-editor?entity=site&id=leadership&lang=en) | **Verified** ✅ |
| **Anchor Scrolling** | Browser Click Jump Verification | `http://127.0.0.1:8000/about/#leadership` | **Fixed & Clear** ✅ |
| **Insights Hero & Digest** | Form Controls + Live Render | [`/insights/`](http://127.0.0.1:8000/insights/) & [CMS](http://127.0.0.1:8090/cms.html#/block-editor?entity=site&id=insights-hero&lang=en) | **Verified (40 Saves)** ✅ |
| **Line Card Module** | Icon & Gated Download Inputs | [`/insights/`](http://127.0.0.1:8000/insights/) & [CMS](http://127.0.0.1:8090/cms.html#/block-editor?entity=site&id=insights-linecard&lang=en) | **Verified (45 Saves)** ✅ |
| **13 Insight Articles** | Visual Inspector & Article View | [`/insights/authorized-it-distributors-uae/`](http://127.0.0.1:8000/insights/authorized-it-distributors-uae/) | **100% Dynamic** ✅ |
| **Arabic 1:1 Parity** | Dual-Language RTL Audit | [`/ar/about/`](http://127.0.0.1:8000/ar/about/) & [`/ar/insights/`](http://127.0.0.1:8000/ar/insights/) | **100% Parity** ✅ |
| **Zero-JSON Mandate** | Full CMS UI Inspection | All Portal Inspectors (`cms.js`) | **0 JSON Textareas** ✅ |

