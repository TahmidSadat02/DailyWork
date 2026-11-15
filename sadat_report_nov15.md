# Daily Work Report — Tahmid Sadat

**Date:** 15 November 2026  
**Employee:** Tahmid Sadat  
**Designation:** Senior QA Engineer & Adversarial Security Specialist  
**Branch:** `sadat` (Clean tree, strict read-only audit — 0 unauthorized code/config modifications)  
**Target Environments:**
- Public Website: [http://182.160.112.72:8000/](http://182.160.112.72:8000/) & Arabic Twin `/ar/`
- Admin CMS Portal: [http://182.160.112.72:8000/cms/](http://182.160.112.72:8000/cms/)
- Remote PostgreSQL: `182.160.112.66:5434/simal-cms`  
**Repository Push Target:** [https://github.com/TahmidSadat02/DailyWork.git](https://github.com/TahmidSadat02/DailyWork.git)  
**Status:** Completed & Verified  

---

## 📌 Executive Summary

Today's operational QA testing on **15 November 2026** conducted an exhaustive, multi-tier audit dedicated to the **Product Management, Catalog Ecosystem, and Hardware/Software Storefronts** across both public client storefronts (`/products/`, `/ar/products/`, `/products/[slug]/`, `/software/products/`) and the administrative CMS Studio (`#/products`, `#/product-new`, `#/product-edit`).

The testing systematically covered:
1. **Public Hardware Catalog Traversal & Facet Engine**: Validating all **80 hardware SKUs**, category filters (`internal-ssd`, `portable-ssd`, `memory`, `monitors`, `gaming`, `accessories`, `laptops`, and anomaly `laptop1`), brand facets, and dynamic sorting chips.
2. **Product Details Page (PDP) Forensic Verification**: Auditing interactive components across sample PDPs—photo gallery carousels, thumbnail switching, at-a-glance cards, 5-tab specification matrices, channel pricing RFQ quantity steppers, WhatsApp direct handoffs, and related brand companion recommendations.
3. **Gallery Media & Multi-Image Asset Pipeline**: Auditing product image references against the local filesystem, verifying 77 unique production images, and testing high-resolution multi-angle image sets (including the 4-photo suite for `ugreen-5-in-1-usb-c-hub`).
4. **Bilingual Arabic Storefront & 1:1 Design Parity (Rule 7)**: Re-testing the Arabic catalog (`/ar/products/`) and verifying the persistent architectural failure where Arabic users are evicted into English PDPs due to the complete lack of `apps/site/src/pages/ar/products/[slug].astro`.
5. **Software Products Vertical & Multi-Platform Catalog**: Auditing the 20 software products in `content_blocks` (40 records across EN/AR), uncovering missing route templates for both the software products hub index and the Arabic software product detail pages.
6. **CMS Studio Product Governance & Database Hygiene**: Auditing product creation, deep specification editing, revision auditing, and identifying a rogue test record (`test-dev`) that contaminates the public catalog with invalid categories and broken image references.

### Summary Metrics of Today's Audit:
| Metric / Testing Scope | Verified Value / Status |
| :--- | :---: |
| **Total Hardware SKUs Audited in Catalog** | **80 SKUs** (Live in PostgreSQL `products` table) |
| **Enterprise Software Products Audited** | **20 Platforms** (40 records in `content_blocks`: 20 EN, 20 AR) |
| **Hardware Catalog Categories Evaluated** | **8 Categories** (`accessories`, `internal-ssd`, `portable-ssd`, `gaming`, `memory`, `monitors`, `laptops`, + anomaly `laptop1`) |
| **Product Media Filesystem Audit** | **80 Products Evaluated** (77 valid unique assets, 3 missing on test record) |
| **Multi-Image Gallery Coverage** | **9 / 80 Products (11.3%)** with multiple photos; **71 / 80 Products (88.7%)** single-photo |
| **PDP Specification Mini-Tabs Tested** | **5 Tabs** (Specifications, Overview, Features, In the Box, Documentation) |
| **Total Verified Defects Cataloged** | **14 Issues** (4 Critical, 4 High, 4 Medium, 2 Low) |
| **Defect Status Tracking** | **10 Carried Forward from Nov 8** (All Unresolved), **4 Newly Discovered Today** |
| **Public vs Arabic Bilingual Parity** | **FAILED (Critical Disparity)** — Missing Arabic PDP and Arabic Software templates |
| **Code / Config / DB Modifications** | **Strict Zero (0)** — Read-Only QA Mandate strictly upheld |

---

## 1. 🛒 Public Hardware Catalog Audit (`/products/`)

The public catalog displays 80 enterprise and consumer IT hardware products across 19 authorized brand lines and specialty hardware categories.

### 1.1 Facet Rail & Client-Side Filtering
- **Category Filter Chips (`#f-cat`):** Verified filtering across 8 active hardware categories (`accessories` [35], `internal-ssd` [15], `portable-ssd` [12], `gaming` [6], `memory` [4], `monitors` [4], `laptops` [3], and the anomaly `laptop1` [1]).
- **Brand Facet Checkboxes (`#f-brand`):** Verified facet list reflecting all authorized brands (Crucial [19], UGREEN [16], Honeywell [13], Arktek [3], Koorui [3], Aiwa [3], Kingston [2], Toshiba [2], Dell [1], HP [1], MSI [1], Hiksemi [1], Nearity [1], Sandisk [1], Western Digital [1], Lenovo [1], Samsung [1], TeamGroup [1], Other [9]). Multi-brand selection correctly calculates intersection and SKU counts.
- **Stock Availability Filter (`#f-stock`):** Toggling "In stock only" preserves all 80 SKUs, reflecting 100% active inventory fulfilled from the JAFZA bonded logistics facility.
- **Sorting Mechanism (`#sortbar`):**
  - `Featured`: Preserves default editorial sort order.
  - `Newest`: Correctly prioritizes products with `is_new: true` (e.g. Crucial P510 NVMe Gen5 SSD).
  - `Brand A–Z`: Correctly groups cards alphabetically by brand identifier.
  - `Name A–Z`: Correctly alphabetizes cards based on `data-name`.
- **Pagination Engine (`#pager`):** Configured for 12 items per page (`PER = 12`). Verified clean pagination controls (7 pages total for 80 items) with instant page jumps and count updates (`#count-note`).

### 1.2 Category Buying Guides (Dynamic PostgreSQL Integration)
- Verified dynamic database binding against PostgreSQL `category_guides` table.
- Clicking category chips populates the buying guide card with introduction copy and key technical points (e.g., PCIe Gen5 speeds up to 12,400 MB/s for internal SSDs; MOV multi-stage surge suppression for Honeywell surge protectors).

### 1.3 Persistent Link Penalty (`PRD-04` Re-verification)
- Verified that lines 112 and 120 of `apps/site/src/pages/products/index.astro` still omit the trailing slash:
  ```astro
  <a class="go" href={`/products/${p.slug}`} aria-label={`View details — ${p.name}`}></a>
  <h3><a href={`/products/${p.slug}`} style="color:inherit;text-decoration:none">{p.name}</a></h3>
  ```
- **Observed Behavior:** Every single click on all 80 product cards generates an unnecessary **HTTP 301 Moved Permanently** redirect to `/products/${p.slug}/`, causing doubled round-trip latency and degrading SEO crawl efficiency. Defect **PRD-04 remains active**.

---

## 2. 🔍 Product Details Page (PDP) Forensic Audit (`/products/[slug]/`)

Conducted exhaustive end-to-end testing across 10 sample PDP routes representing each hardware category (e.g., `crucial-p510-nvme-ssd`, `crucial-t705-ssd`, `crucial-ddr5-pro-memory`, `ugreen-5-in-1-usb-c-hub`, `ugreen-revodok-pro-313-13-in-1-usb-c-docking-sta-0`, `aiwa-mf240e-v-flat-slim-monitor-0`, `honeywell-surge-protector-4-universal-sockets`).

### 2.1 Visual Gallery, Carousel & Multi-Image Pipeline
- **Hero Image & Thumbnails (`#gal` & `#thumbs`):**
  - On products with multiple images (e.g., `crucial-p510-nvme-ssd` with 3 images, `koorui-27-inch-gaming-monitor` with 3 images), clicking thumbnail buttons switches the primary photo instantly without layout jitter.
  - Keyboard navigation and focus-visible outlines comply with WCAG 2.1 accessibility guidelines.
- **Media Asset Audit & Multi-Image Inconsistency (`PRD-14`):**
  - Querying the database revealed that **71 out of 80 products (88.7%)** contain only a single photo in their `imgs` array.
  - On single-photo products, the thumbnail strip (`#thumbs`) is completely omitted (`imgs.length > 1`), leaving potential B2B buyers unable to inspect ports, packaging, or connector pinouts.
  - Audited the newly prepared 4-photo high-resolution studio suite for `ugreen-5-in-1-usb-c-hub` (`ugreen-5-in-1-usb-c-hub-0.png` through `ugreen-5-in-1-usb-c-hub-3.png`) across `apps/site`, `apps/portal`, and `prototype`. Updating the product's database record to reference all 4 images activates the full thumbnail carousel seamlessly.

### 2.2 Commercial Action Box & Stepper
- **Channel Pricing Notice:** Clearly indicates `"RFQ — no list price"`, establishing transparent expectations for volume B2B trade buyers.
- **Quantity Stepper (`#q-inc`, `#q-dec`, `#q-n`):** Controls quantity increment/decrement. Manual text entry allows non-numeric characters if unvalidated (`PRD-06` remains open).
- **WhatsApp Integration (`#wa`):** Generates pre-populated WhatsApp intent link targeting `+971 50 848 7861` with SKU and product name.
- **Add to RFQ Basket (`#add`):** Interacts with `add-rfq.js`, serializing the item into `localStorage.simal_rfq_basket` and animating the header `#rfqCount` pill from `0` to `1`.

### 2.3 Five-Tab Technical Specification Matrix
Verified tab switching behavior and panel visibility:
1. **Specifications (`#tab-specs`):** Complete key-value specification table querying `product_deep.specs` and fallback `products.specs`.
2. **Overview (`#tab-desc`):** Renders detailed narrative description (`products.descr`).
3. **Features & Use Cases (`#tab-feat`):** Split layout rendering bulleted lists for "Key features" (`product_deep.feats`) and "Best for" enterprise workloads (`product_deep.use_cases`).
4. **In the Box & Accessories (`#tab-box`):** Package contents listing paired with dynamic companion accessories queried from the `accessories` category.
5. **Documentation (`#tab-docs`):** Download links for official manufacturer datasheets, warranty terms, and channel buying guidelines.

---

## 3. 🚨 Bilingual Arabic Storefront Disparity & Architectural Breakdown (`/ar/products/`)

Auditing the Arabic storefront confirmed that the **Critical Architectural Disparities reported on Nov 8 remain completely unaddressed**:

### 3.1 `PRD-01` (Critical — Unresolved): Arabic Catalog Links Evict Users into English PDPs
- **Location:** [http://182.160.112.72:8000/ar/products/](http://182.160.112.72:8000/ar/products/)
- **Defect:** In `apps/site/src/pages/ar/products/index.astro`, all 80 product card anchors continue to be hardcoded to English PDP URLs:
  ```astro
  <a class="go" href={`/products/${p.slug}/`} aria-label={p.name}></a>
  <h4><a href={`/products/${p.slug}/`} style="color:var(--ink);text-decoration:none">{p.name}</a></h4>
  ```
- **Observed Behavior:** An Arabic-speaking corporate client browsing the Arabic catalog clicks a product and is instantly ejected from the Arabic experience into the English PDP (`/products/[slug]/`), losing the Arabic reading direction (`dir="rtl"`), Arabic navigation bar, and localized breadcrumbs.
- **Severity:** **Critical (Direct Violation of Rule 7 — Mandatory 1:1 Design Parity)**

### 3.2 `PRD-02` (Critical — Unresolved): Complete Absence of Arabic PDP Route Template
- **Location:** `apps/site/src/pages/ar/products/[slug].astro`
- **Defect:** The file `apps/site/src/pages/ar/products/[slug].astro` **does not exist** in the repository.
- **Observed Behavior:** Any attempt to access a localized product details URL (e.g., `http://182.160.112.72:8000/ar/products/ugreen-5-in-1-usb-c-hub/`) immediately returns **HTTP 404 Not Found**. The entire Arabic product details page experience is completely missing from the platform.
- **Severity:** **Critical**

### 3.3 `PRD-03` (High — Unresolved): PDP Language Switcher Misroutes to Catalog Root
- **Location:** Header Language Switcher on any English PDP (`/products/[slug]/`)
- **Defect:** In `apps/site/src/layouts/BaseLayout.astro` (lines 218–219):
  ```javascript
  else if (cleanPath.startsWith('/products/') || cleanPath.startsWith('/brands/') || cleanPath.startsWith('/compare/')) {
    arPath = '/ar/products/';
  }
  ```
- **Observed Behavior:** When viewing a product on an English PDP and clicking the **"العربية"** language toggle, the user is redirected to `/ar/products/` (the catalog root) instead of preserving the specific product context.

---

## 4. 💻 Vertical 4: Software Products Vertical & Detail Pages Audit

The Software Products ecosystem represents Simal's enterprise software practice powered by Unisoft Systems Ltd, encompassing 20 specialized ERPs and business platforms.

### 4.1 English Software Product Detail Pages (`/software/products/[slug].astro`)
- **Database Binding:** Dynamically queries `content_blocks` where `entity = 'software_product'` and `lang = 'en'` (20 active records: `asset-management`, `cattle-management`, `collection-recovery`, `courier-management`, `distribution-management`, `export-import-management`, `financial-management`, `freight-forwarding`, `healthcare-management`, `hotel-management`, `human-resource-management`, `inventory-management`, `manufacturing-management`, `point-of-sale`, `procurement-management`, `real-estate-management`, `restaurant-management`, `school-management`, `transport-management`, `vat-tax-management`).
- **Visual Presentation:** Clean hero layout with product icon, title, tagline, flagship star badge, target sector chips, and feature breakdown cards.
- **Related Platforms:** Automatically suggests 4 related software platforms from the Unisoft portfolio.

### 4.2 New Defect Discoveries in Software Products:
- **`PRD-12` (High — Software Products Hub Missing Index Route):**  
  Direct requests to `http://182.160.112.72:8000/software/products/` return **HTTP 404 Not Found**. The directory `apps/site/src/pages/software/products/` only contains `[slug].astro` and lacks an `index.astro` catalog file. Visitors navigating via breadcrumbs or direct URL are met with a broken endpoint.
- **`PRD-13` (Critical — Complete Absence of Arabic Software Product Routes):**  
  While the database contains **20 authentic Arabic records** in `content_blocks` (`entity = 'software_product'`, `lang = 'ar'`), there is no corresponding template `apps/site/src/pages/ar/software/products/[slug].astro`. Arabic corporate clients cannot access any localized software product pages.

---

## 5. 🎛️ Portal CMS Studio Products Management & Database Hygiene (`#/products`)

Audited the administrative catalog management interface inside the Portal CMS (`/cms/`).

### 5.1 Products Grid & Telemetry (`viewProductsCatalog`)
- **KPI Badges:** Live count accurately reflects **80 products**, displaying counts for In Stock items, Backorder items, and New Arrivals.
- **Search & Filter Controls:** Real-time search (`#prod-filter-search`), stock status dropdown (`#prod-filter-stock`), and brand filter buttons filter cards instantly without page reloads.
- **Strict Zero-JSON Mandate (Rule 4):** Form fields strictly utilize structured visual inputs:
  - Single-line text inputs for Name, Slug, SKU, Brand, Category, and Warranty.
  - Multi-line textareas for Short Description and Full Narrative Description.
  - Interactive specification matrix repeater (`collectPageSpecs()`) with dynamic `＋ Add Specification Row` and remove buttons.
  - Media asset selector for primary and gallery images.

### 5.2 Critical Database Contamination (`PRD-11`): Rogue Test Product
- **Discovery:** A database query across the `products` table revealed an illegitimate demo record:
  - **Slug:** `test-dev`
  - **SKU:** `c2026`
  - **Name:** `test DEV`
  - **Brand:** `kingston`
  - **Category:** `laptop1` (Non-existent category causing facet pollution)
  - **Category Label:** `dell laptop` (Contradictory brand pairing)
  - **Images:** `["assets/img/products/1error.png", "assets/img/products/banner1.jpg", "assets/img/products/digital equipment hero-1.jpeg"]`
- **Impact:**
  - Filesystem audit confirmed that all 3 image files (`1error.png`, `banner1.jpg`, `digital equipment hero-1.jpeg`) **do not exist** on disk, causing broken image icons.
  - The invalid category `laptop1` creates an orphaned filter chip in the public catalog.
  - Violates production hygiene standards and clutters the public catalog.
- **Severity:** **High (Requires Immediate SQL Cleanup)**

---

## 6. 📋 Master Product Defect Registry (14 Verified Issues)

The table below itemizes all **14 verified defects** tracked during today's product ecosystem audit:

| Bug ID | Category | Target Location | Severity | Defect Summary & Observed Impact | Status | Recommended Engineering Fix |
| :--- | :--- | :--- | :---: | :--- | :---: | :--- |
| **PRD-01** | Bilingual Parity | `/ar/products/` | **Critical** | Arabic catalog links point directly to English PDPs (`/products/${p.slug}/`), evicting Arabic users into English. | **Open** | Change card anchors in `ar/products/index.astro` to `/ar/products/${p.slug}/`. |
| **PRD-02** | Missing Route | `/ar/products/[slug]/` | **Critical** | `ar/products/[slug].astro` does not exist; all direct Arabic PDP URLs return HTTP 404. | **Open** | Create `apps/site/src/pages/ar/products/[slug].astro` porting English PDP with RTL layout. |
| **PRD-03** | Navigation / UX | `/products/[slug]/` (Lang) | **High** | Header language toggle on PDP redirects to `/ar/products/` catalog root instead of Arabic PDP. | **Open** | Update `BaseLayout.astro` to map `/products/[slug]/` directly to `/ar/products/[slug]/`. |
| **PRD-04** | Performance / SEO | `/products/` Card Links | **Medium** | All 80 product links lack trailing slash (`/products/${p.slug}`), triggering HTTP 301 redirects. | **Open** | Append trailing slash `/products/${p.slug}/` in `apps/site/src/pages/products/index.astro`. |
| **PRD-05** | Architecture / SSG | CMS `#/product-edit` | **Medium** | Saving product updates in CMS does not trigger an SSG build hook, leaving static live site stale. | **Open** | Add an outbox webhook trigger in `cms.service.js` upon `updateProduct` / `createProduct`. |
| **PRD-06** | Form Validation | `/products/[slug]/` (Qty) | **Medium** | Quantity input allows non-numeric and negative values via manual text entry without clamping. | **Open** | Add `type="number" min="1" max="999"` and `onblur` clamping on `#q-n`. |
| **PRD-07** | UX / Mobile | `/products/[slug]/` (WA) | **Low** | WhatsApp intent URL includes lengthy unformatted slug for products without official SKU. | **Open** | Fallback to sanitized short title instead of raw slug hash in WhatsApp message body. |
| **PRD-08** | Data Model | Database `products` Table | **Medium** | PostgreSQL `products` table lacks localized columns (`name_ar`, `short_ar`, `specs_ar`). | **Open** | Add localized column migrations in `database/sql/` to enable authentic Arabic specs. |
| **PRD-09** | Layout Stability | `/products/` Buying Guide | **Low** | Switching category chips causes buying guide card to jump without CSS transition, inducing CLS. | **Open** | Add `min-height` and CSS opacity transition on `#guide-card`. |
| **PRD-10** | SEO / Schema | `/products/` Schema | **Low** | English catalog omits `BreadcrumbList` schema in frontmatter while Arabic catalog includes it. | **Open** | Add `BreadcrumbList` JSON-LD schema into `apps/site/src/pages/products/index.astro`. |
| **PRD-11** | Data Hygiene | PostgreSQL `products` | **High** | Rogue test product `test-dev` with category `laptop1` and 3 broken images contaminates catalog. | **NEW** | Create SQL cleanup script in `database/sql/` to delete `test-dev` from `products` and `product_deep`. |
| **PRD-12** | Missing Route | `/software/products/` | **High** | Direct access to `/software/products/` returns HTTP 404 because directory lacks `index.astro`. | **NEW** | Create `apps/site/src/pages/software/products/index.astro` software portfolio directory. |
| **PRD-13** | Missing Route | `/ar/software/products/*` | **Critical** | 20 Arabic software product records in DB have no matching Astro template (HTTP 404 on all URLs). | **NEW** | Create `apps/site/src/pages/ar/software/products/[slug].astro` for localized software details. |
| **PRD-14** | Content / Asset | Hardware Catalog PDPs | **Medium** | 88.7% (71/80) of hardware SKUs contain only 1 image, disabling the PDP thumbnail gallery. | **NEW** | Populate multi-angle studio asset suites for key SKUs (following `ugreen-5-in-1-usb-c-hub`). |

---

## 7. 🛠️ Actionable Remediation Steps for Sprint Team

1. **Immediate P0 Action (Arabic Hardware & Software PDP Bring-Up):**
   - Create `apps/site/src/pages/ar/products/[slug].astro` mirroring English PDP with `lang="ar"` and `dir="rtl"`.
   - Update `apps/site/src/pages/ar/products/index.astro` card links to point to `/ar/products/${p.slug}/`.
   - Create `apps/site/src/pages/ar/software/products/[slug].astro` to expose the 20 seeded Arabic software products.
   - Update `BaseLayout.astro` language switcher mappings to maintain slug continuity across both languages.

2. **Immediate P1 Action (Database Sanitization & Test Data Removal):**
   - Author and execute direct SQL script `database/sql/61_remove_test_dev_product.sql`:
     ```sql
     BEGIN;
     DELETE FROM product_deep WHERE slug = 'test-dev';
     DELETE FROM products WHERE slug = 'test-dev';
     COMMIT;
     ```
   - Eliminate category pollution (`laptop1`) and 3 broken image references from the live catalog.

3. **Immediate P2 Action (SEO Trailing Slash Fix & Software Hub Index):**
   - Update lines 112 and 120 of `apps/site/src/pages/products/index.astro` to `href={`/products/${p.slug}/`}` to eliminate all 80 HTTP 301 redirects.
   - Create `apps/site/src/pages/software/products/index.astro` to resolve the HTTP 404 on the software products catalog endpoint.

---

**Report Authored By:** Tahmid Sadat  
**Repository Branch:** `sadat`  
**Execution Timestamp:** 15 November 2026, 18:25 GMT+6  
**Git Push Remote:** `https://github.com/TahmidSadat02/DailyWork.git`
