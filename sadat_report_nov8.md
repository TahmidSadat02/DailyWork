# Daily Work Report — Tahmid Sadat

**Date:** 08 November 2026  
**Employee:** Tahmid Sadat  
**Designation:** Senior QA Engineer & Adversarial Security Specialist  
**Branch:** `sadat` (Clean tree, strict read-only audit — 0 code/config modifications)  
**Target Environments:**
- Public Website: [http://182.160.112.72:8000/](http://182.160.112.72:8000/) & Arabic Twin `/ar/`
- Admin CMS Portal: [http://182.160.112.72:8000/cms/](http://182.160.112.72:8000/cms/)
- Remote PostgreSQL: `182.160.112.66:5434/simal-cms`  
**Repository Push Target:** [https://github.com/TahmidSadat02/DailyWork.git](https://github.com/TahmidSadat02/DailyWork.git)  
**Status:** Completed & Verified  

---

## 📌 Executive Summary

Today's operational QA testing on **08 November 2026** conducted a comprehensive, forensic deep-dive specifically dedicated to the **Product Management & Hardware Catalog Ecosystem** across both public client storefronts (`/products/`, `/ar/products/`, `/products/[slug]/`) and the administrative CMS Studio (`#/products`, `#/product-new`, `#/product-edit`).

The testing encompassed:
1. **Full Catalog Traversal & Facet Engine**: Auditing all **80 hardware SKUs**, category filters (`internal-ssd`, `portable-ssd`, `memory`, `monitors`, `gaming`, `accessories`, `laptops`, `surge-protectors`), brand facets, and dynamic sorting chips.
2. **Product Details Page (PDP) Experience**: Auditing interactive components across sample PDPs—photo gallery carousels, at-a-glance cards, 5-tab specification matrices, channel pricing RFQ quantity steppers, WhatsApp direct handoffs, and related brand companion recommendations.
3. **Bilingual Arabic Catalog & Design Parity**: Uncovering a **critical architectural gap** where clicking any product card in the Arabic catalog (`/ar/products/`) evicts the user directly into the English PDP, caused by the complete absence of an Arabic PDP route (`/ar/products/[slug].astro`).
4. **Media & Image Asset Integrity**: Auditing all 77 unique product image URLs on the live catalog; confirming resolution of image 404s via `.webp` asset updates, while uncovering trailing slash redirect penalties across all 80 catalog cards.
5. **CMS Products Lifecycle & SSG Synchronization**: Auditing product creation, deep specification editing, revision auditing, and testing public site cache propagation latency.

### Summary Metrics of Today's Product Audit:
| Metric / Testing Scope | Verified Value / Status |
| :--- | :---: |
| **Total Hardware SKUs Audited in Catalog** | **80 SKUs** (Live in PostgreSQL `products` table) |
| **Catalog Pages / Views Evaluated** | **4 Core Views** (`/products/`, `/ar/products/`, PDPs, CMS Products) |
| **Product Image Assets Verified on Live Catalog** | **77 Unique Images** (100% active, 0 HTTP 404s) |
| **PDP Specification Mini-Tabs Tested** | **5 Tabs** (Specs, Overview, Features, In the Box, Docs) |
| **Total Product-Specific Defects Cataloged** | **10 Issues** (2 Critical, 2 High, 4 Medium, 2 Low) |
| **Public vs Arabic Bilingual Parity** | **FAILED (Critical Disparity)** — Missing Arabic PDP template |
| **Code / Config / DB Modifications** | **Strict Zero (0)** — Read-Only QA Mandate strictly upheld |

---

## 1. 🛒 Public Hardware Catalog Audit (`/products/`)

The public catalog displays 80 enterprise and consumer IT hardware products across 21 authorized brand lines.

### 1.1 Facet Rail & Client-Side Filtering
- **Category Filter Chips:** Verified 8 categories (`All`, `internal-ssd`, `portable-ssd`, `memory`, `monitors`, `gaming`, `accessories`, `laptops`, `surge-protectors`). Selecting any category filters the DOM grid instantly via `catalog-island.js` and updates the URL hash (e.g. `/products/#memory`).
- **Brand Checkboxes:** Verified facet list reflecting all 21 authorized brands (Crucial, Dell, HP, Hiksemi, Honeywell, Kingston, Koorui, Lenovo, MSI, Nearity, Sandisk, Toshiba, UGREEN, Western Digital, etc.). Multi-brand selection correctly calculates intersection and SKU counts.
- **Stock Availability Filter (`#f-stock`):** Toggling "In stock only" reduces the 80 SKUs to active stock items fulfilled from the JAFZA bonded logistics facility.
- **Sorting Mechanism (`#sortbar`):**
  - `Featured`: Default editorial sort order.
  - `Name A–Z`: Correctly alphabetizes cards based on `data-name`.
  - `Newest`: Filters and elevates SKUs carrying the `is_new: true` flag (e.g. Crucial P510 NVMe Gen5 SSD).
- **Pagination Engine:** Configured for 12 items per page (`PER = 12`). Verified clean pagination controls (7 pages total for 80 items) with instant page jumps and count updates (`count-note`).

### 1.2 Category Buying Guides (Dynamic PostgreSQL Integration)
- Verified dynamic database binding against PostgreSQL `category_guides` table.
- Clicking category chips populates the buying guide card with introduction copy and key technical points (e.g., PCIe Gen5 speeds up to 12,400 MB/s for internal SSDs; MOV multi-stage surge suppression for Honeywell surge protectors).

---

## 2. 🔍 Product Details Page (PDP) Forensic Audit (`/products/[slug]/`)

Conducted exhaustive end-to-end testing across 10 sample PDP routes representing each hardware category (e.g., `crucial-p510-nvme-ssd`, `crucial-t705-ssd`, `crucial-ddr5-pro-memory`, `ugreen-revodok-pro-313-13-in-1-usb-c-docking-sta-0`, `aiwa-mf240e-v-flat-slim-monitor-0`, `honeywell-surge-protector-4-universal-sockets`).

### 2.1 Visual Elements & Gallery
- **Hero Image & Thumbnails (`#gal` & `#thumbs`):** Thumbnail click switches the primary photo instantly without page jitter. Focus-visible outline complies with WCAG accessibility guidelines.
- **At-a-Glance Card (`#glance`):** Accurately renders top 4 technical specifications parsed from `product_deep.specs` alongside local JAFZA stock availability.
- **The Simal Assurance Card (`#assurance`):** Displays authorized distribution credentials, local UAE warranty RMA notice, and direct volume enquiry link to `/contact/`.

### 2.2 Commercial Action Box & Stepper
- **Channel Pricing Notice:** Clearly indicates `"RFQ — no list price"`, setting expectations for volume B2B trade buyers.
- **Quantity Stepper (`#q-inc`, `#q-dec`, `#q-n`):** Controls quantity increment/decrement.
- **WhatsApp Integration (`#wa`):** Generates pre-populated WhatsApp intent link:
  `https://wa.me/971508487861?text=Simal%20RFQ%20%E2%80%94%20Crucial%20P510%20(SKU%20CT2000P510),%20qty%201`. Verified valid target number and properly escaped query text.
- **Add to RFQ Basket (`#add`):** Interacts with `add-rfq.js`, serializing the item into `localStorage.simal_rfq_basket` and animating the header `#rfqCount` pill from `0` to `1`.

### 2.3 Five-Tab Technical Specification Matrix
Verified tab switching behavior and panel visibility:
1. **Specifications (`#tab-specs`):** Complete key-value specification table querying `product_deep.specs` and fallback `products.specs`. Includes brand, SKU, category, local UAE warranty months, and channel RFQ volume tiers.
2. **Overview (`#tab-desc`):** Renders detailed narrative description (`products.descr`).
3. **Features & Use Cases (`#tab-feat`):** Split layout rendering bulleted lists for "Key features" (`product_deep.feats`) and "Best for" enterprise workloads (`product_deep.use_cases`).
4. **In the Box & Accessories (`#tab-box`):** Package contents listing paired with dynamic companion accessories queried from the `accessories` category.
5. **Documentation (`#tab-docs`):** Download links for official manufacturer datasheets, warranty terms, and channel buying guidelines.

---

## 3. 🚨 Critical Defect Discoveries: Bilingual Parity & Routing Failures

Today's audit uncovered severe architectural breakdowns affecting the Arabic store experience and SEO link hygiene:

### 3.1 `PRD-01` (Critical): Arabic Catalog Links Point to English PDPs
- **Location:** [http://182.160.112.72:8000/ar/products/](http://182.160.112.72:8000/ar/products/)
- **Defect:** In `apps/site/src/pages/ar/products/index.astro`, all 80 product card anchors are hardcoded to English PDP URLs:
  ```astro
  <a class="go" href={`/products/${p.slug}/`} aria-label={p.name}></a>
  <h4><a href={`/products/${p.slug}/`} style="color:var(--ink);text-decoration:none">{p.name}</a></h4>
  ```
- **Observed Behavior:** An Arabic-speaking client browsing the Arabic catalog clicks a product and is instantly dumped into the English PDP (`/products/[slug]/`), losing the Arabic reading direction (`dir="rtl"`), Arabic header navigation, and localized breadcrumbs.
- **Severity:** **Critical (Rule 7 Violation — 1:1 Design Parity)**

### 3.2 `PRD-02` (Critical): Complete Absence of Arabic PDP Route Template
- **Location:** `apps/site/src/pages/ar/products/[slug].astro`
- **Defect:** The file `apps/site/src/pages/ar/products/[slug].astro` **does not exist** in the repository.
- **Observed Behavior:** Attempting to visit any localized product URL (e.g., `http://182.160.112.72:8000/ar/products/crucial-p510-nvme-ssd/`) returns **HTTP 404 Not Found**. The entire Arabic product details page experience is unimplemented.
- **Severity:** **Critical**

### 3.3 `PRD-03` (High): PDP Language Switcher Misroutes to Catalog Root
- **Location:** Header Language Switcher on any English PDP (`/products/[slug]/`)
- **Defect:** In `apps/site/src/layouts/BaseLayout.astro` (lines 218–219):
  ```javascript
  else if (cleanPath.startsWith('/products/') || cleanPath.startsWith('/brands/') || cleanPath.startsWith('/compare/')) {
    arPath = '/ar/products/';
  }
  ```
- **Observed Behavior:** When viewing a product on an English PDP and clicking the **"العربية"** language toggle, the user is redirected to `/ar/products/` (the catalog root) instead of the corresponding product details view, completely destroying the browsing journey.
- **Severity:** **High**

### 3.4 `PRD-04` (Medium): 301 Redirect Loop on All 80 Public Catalog Links
- **Location:** [http://182.160.112.72:8000/products/](http://182.160.112.72:8000/products/)
- **Defect:** In `apps/site/src/pages/products/index.astro` (lines 112, 120), product card anchors omit the trailing slash:
  ```astro
  <a class="go" href={`/products/${p.slug}`} aria-label={`View details — ${p.name}`}></a>
  ```
- **Observed Behavior:** Astro SSG generates directory endpoints requiring a trailing slash (`/products/${p.slug}/`). Every single click on all 80 product cards generates an unnecessary **HTTP 301 Moved Permanently** redirect, doubling round-trip latency and degrading SEO crawl efficiency.
- **Severity:** **Medium**

---

## 4. 🎛️ CMS Studio Products Management Audit (`#/products`)

Audited the administrative catalog management interface inside the Portal CMS (`/cms/`).

### 4.1 Products Grid & Telemetry (`viewProductsCatalog`)
- **KPI Badges:** Live count accurately reflects **80 products**, displaying counts for In Stock items, Backorder items, and New Arrivals.
- **Search & Filter Controls:** Tested real-time search input (`#prod-filter-search`), stock status dropdown (`#prod-filter-stock`), and brand filter buttons. Cards filter instantly without page reloads.
- **Export CSV:** Tested `#lp-btn-products` CSV export. Successfully generates a clean, well-structured CSV file containing SKU, Name, Brand, Category, Stock Status, and Updated Timestamp.

### 4.2 Full-Page Product Editor (`#/product-edit?slug=...`)
- **Visual Fields:** Completely conforms to the **Strict Zero-JSON Mandate (Rule 4)**. Form fields provide:
  - Single-line inputs for Name, Slug, SKU, Brand, Category, and Warranty.
  - Multi-line textarea for Short Description and Full Narrative Description.
  - Interactive specification matrix repeater (`collectPageSpecs()`) with dynamic `＋ Add Specification Row` and remove buttons.
  - Media asset selector for primary and gallery images.
- **Revision Tracking:** Saving creates an entry in `cms_revisions` capturing the previous snapshot.
- **`PRD-05` (Medium — SSG Propagation Latency):**  
  Saving changes in CMS updates PostgreSQL table `products` and `product_deep` in real time, but does **not** trigger an Astro SSG rebuild. Updated specifications do not appear on the live website until a manual rebuild occurs.

---

## 5. 📋 Master Product Defect Registry

The table below itemizes all **10 defects** identified during today's product ecosystem audit:

| Bug ID | Category | Target Location | Severity | Defect Summary & Observed Impact | Recommended Engineering Fix |
| :--- | :--- | :--- | :---: | :--- | :--- |
| **PRD-01** | Bilingual Parity | `/ar/products/` | **Critical** | Arabic catalog links point directly to English PDPs (`/products/${p.slug}/`), evicting Arabic users. | Change card anchors in `ar/products/index.astro` to `/ar/products/${p.slug}/`. |
| **PRD-02** | Missing Route | `/ar/products/[slug]/` | **Critical** | `ar/products/[slug].astro` does not exist; all direct Arabic PDP URLs return HTTP 404. | Create `apps/site/src/pages/ar/products/[slug].astro` porting English PDP with RTL layout. |
| **PRD-03** | Navigation / UX | `/products/[slug]/` (Lang) | **High** | Header language toggle on PDP redirects to `/ar/products/` catalog root instead of Arabic PDP. | Update `BaseLayout.astro` to map `/products/[slug]/` directly to `/ar/products/[slug]/`. |
| **PRD-04** | Performance / SEO | `/products/` Card Links | **Medium** | All 80 product links lack trailing slash (`/products/${p.slug}`), triggering HTTP 301 redirects. | Append trailing slash `/products/${p.slug}/` in `apps/site/src/pages/products/index.astro`. |
| **PRD-05** | Architecture / SSG | CMS `#/product-edit` | **Medium** | Saving product updates in CMS does not trigger an SSG build hook, leaving static live site stale. | Add an outbox webhook trigger in `cms.service.js` upon `updateProduct` / `createProduct`. |
| **PRD-06** | Form Validation | `/products/[slug]/` (Qty) | **Medium** | Quantity input allows non-numeric and negative values via manual text entry without clamping. | Add `type="number" min="1" max="999"` and `onblur` clamping on `#q-n`. |
| **PRD-07** | UX / Mobile | `/products/[slug]/` (WA) | **Low** | WhatsApp intent URL includes lengthy unformatted slug for products without official SKU. | Fallback to sanitized short title instead of raw slug hash in WhatsApp message body. |
| **PRD-08** | Data Model | Database `products` Table | **Medium** | PostgreSQL `products` table lacks localized columns (`name_ar`, `short_ar`, `specs_ar`). | Add localized column migrations in `database/sql/` to enable authentic Arabic specs. |
| **PRD-09** | Layout Stability | `/products/` Buying Guide | **Low** | Switching category chips causes buying guide card to jump without CSS transition, inducing CLS. | Add `min-height` and CSS opacity transition on `#guide-card`. |
| **PRD-10** | SEO / Schema | `/products/` Schema | **Low** | English catalog omits `BreadcrumbList` schema in frontmatter while Arabic catalog includes it. | Add `BreadcrumbList` JSON-LD schema into `apps/site/src/pages/products/index.astro`. |

---

## 6. 🛠️ Actionable Remediation Steps for Sprint Team

1. **Immediate P0 Action (Arabic PDP Bring-Up):**
   - Duplicate `apps/site/src/pages/products/[slug].astro` into `apps/site/src/pages/ar/products/[slug].astro`.
   - Set `lang="ar"` and `dir="rtl"` in `BaseLayout`.
   - Update `ar/products/index.astro` card links to point to `/ar/products/${p.slug}/`.
   - Update `BaseLayout.astro` twin root mapping so switching languages preserves the exact product slug.

2. **Immediate P1 Action (Link & SEO Trailing Slashes):**
   - Update lines 112 and 120 of `apps/site/src/pages/products/index.astro` to add trailing slashes: `href={`/products/${p.slug}/`}`.
   - Eliminate all 80 unnecessary HTTP 301 redirects across the catalog.

3. **Immediate P2 Action (Quantity Stepper Hardening):**
   - In `apps/site/public/assets/js/pdp-island.js`, add an `input` event listener validating that `parseInt(qInp.value, 10) >= 1`, preventing invalid RFQ basket payloads.

---

**Report Authored By:** Tahmid Sadat  
**Repository Branch:** `sadat`  
**Execution Timestamp:** 08 November 2026, 18:22 GMT+6  
**Git Push Remote:** `https://github.com/TahmidSadat02/DailyWork.git`
