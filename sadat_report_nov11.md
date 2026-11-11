# Daily Work Report — Tahmid Sadat

**Date:** 11 November 2026  
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

Today's operational QA testing on **11 November 2026** was focused on three critical business pillars of the Simal platform:
1. **Software Services & UniERP Ecosystem** (`/software/`, `/ar/software/`, and all specialized subpages)
2. **Enterprise IT Solutions & AMC Maintenance SLAs** (`/solutions/`, `/ar/solutions/`, and individual detail pages)
3. **Research, Insights & Editorial Newsroom** (`/insights/`, `/ar/insights/`, and all 13 published articles)

The audit systematically tested public client interfaces, interactive calculators, modal lead captures, Arabic RTL layout parity, link integrity, and administrative management within Portal CMS Studio (`#/software-suites`, `#/it-solutions`, `#/amc-tiers`, `#/insights`).

### Summary Metrics of Today's Audit:
| Metric / Testing Scope | Verified Value / Status |
| :--- | :---: |
| **Hub Pages Audited** | **6 Hubs** (`/software/`, `/ar/software/`, `/solutions/`, `/ar/solutions/`, `/insights/`, `/ar/insights/`) |
| **Enterprise IT Solution Lines Tested** | **12 Lines** (12/12 active on EN; 0/12 on AR due to missing template) |
| **Published Insight Articles Tested** | **13 Articles** (13/13 active on EN; 13/13 active on AR) |
| **Interactive Calculators Evaluated** | **1 System** (UniERP Interactive ROI Calculator) |
| **Total Verified Defects Discovered Today** | **12 Issues** (3 Critical, 3 High, 4 Medium, 2 Low) |
| **Code / Config / DB Modifications** | **Strict Zero (0)** — Read-Only QA Mandate strictly upheld |

---

## 1. 🧑‍💻 Vertical 1: Software Services & UniERP Ecosystem Audit

The Software Services vertical represents Simal's enterprise software practice powered by UniSoft Systems (ISO 9001, ISO 27001 certified), focusing on the flagship **UniERP** (Odoo 19 Community Edition customized for GCC e-invoicing and enterprise governance).

### 1.1 English Software Hub (`/software/`)
- **Hero & Flagship Banner:** Accurately highlights UniERP zero-license-cost proposition ($0/user license fee), UAE FTA Peppol e-invoicing compliance (2027 mandate), and enterprise SLA guarantees.
- **Interactive ROI Calculator (`#roi`):**
  - Inputs: Industry select, Company size (11–2,000+ employees), Current monthly license cost ($90 default), ERP user seats (60 default), IT admin hours (80/mo), and Manual process hours (160/mo).
  - Output: Successfully calculates dynamic annual savings (`#r-total`: **$94,800/yr** default), breaking down License elimination ($64,800/yr) and IT admin automation ($8,400/yr).
  - Validation: Clamps negative inputs and dynamically re-renders without full page reloads.
- **Enterprise Solutions & Subservices Directory:**
  - Tested links to `/software/solutions/univat/` (UniVAT NBR-approved VAT compliance engine) — **HTTP 200 OK**.
  - Software Products Directory: 10 ERP products cataloged and accessible.
  - Subservice detail routes: `/software/services/[slug]/` and `/software/modules/[slug]/`.

### 1.2 Arabic Software Hub Disparities (`/ar/software/`)
- **`SFT-01` (Critical — Complete Absence of Interactive ROI Calculator):**  
  While the English `/software/` page features the high-converting ROI calculator, the entire section is **completely missing** from `apps/site/src/pages/ar/software/index.astro`. Arabic corporate buyers have zero access to the savings calculation tool, directly violating Rule 7 (Mandatory 1:1 Design Parity).
- **`SFT-02` (Critical — Missing Arabic Software Subservice Routes):**  
  In `apps/site/src/pages/ar/software/`, only `index.astro` exists. The entire directory structure for specialized subservices (`modules/`, `services/`, `products/`, `solutions/`, `industries/`) is unimplemented for Arabic, causing HTTP 404 errors on localized subpages.
- **`SFT-03` (Medium — Demo Form Unsanitized Input):**  
  Submitting raw HTML tags (`<b>`, `<script>`) in the software demo request form is accepted by `/api/contact` without sanitization warnings.

---

## 2. 🛠️ Vertical 2: Enterprise IT Solutions & AMC Maintenance SLAs Audit

The IT Solutions vertical covers 12 core enterprise infrastructure practices, structured cabling, cybersecurity, and Annual Maintenance Contracts (AMC).

### 2.1 English IT Solutions Hub & Detail Pages (`/solutions/` & `[id].astro`)
- **12 Service Lines Audited:**
  1. `/solutions/amc/` — Annual Maintenance Contracts & Hardware SLAs (HTTP 200)
  2. `/solutions/av/` — Audio-Visual & Boardroom Collaboration (HTTP 200)
  3. `/solutions/cabling/` — Structured Cabling & Passive Fiber Infrastructure (HTTP 200)
  4. `/solutions/cloud-security/` — Cloud Security, Backup & Disaster Recovery (HTTP 200)
  5. `/solutions/data-recovery/` — Enterprise Storage & Clean-Room Data Recovery (HTTP 200)
  6. `/solutions/firewall/` — Next-Gen UTM Firewalls & Perimeter Defense (HTTP 200)
  7. `/solutions/infra/` — Datacenter Server Racks & Power Conditioning (HTTP 200)
  8. `/solutions/licensing/` — Microsoft 365 & Tier-1 Software Licensing (HTTP 200)
  9. `/solutions/managed/` — Fully Managed IT Support & Helpdesk Operations (HTTP 200)
  10. `/solutions/network-security/` — Core Switching, Routing & SD-WAN Architecture (HTTP 200)
  11. `/solutions/support/` — On-Site Field Engineering & Rapid Response (HTTP 200)
  12. `/solutions/telephony/` — IP PBX, VoIP & Cloud Unified Communications (HTTP 200)
- **Detail Page Components:**
  - 5-icon rotation framework (`🎯`, `⚙️`, `📊`, `🔗`, `🧭`) displaying structured scope deliverables.
  - AMC Maintenance SLAs table dynamically querying `content_blocks` (`amc-tiers`).
  - Hardware cross-navigation chips linking relevant categories (`monitors`, `accessories`, `internal-ssd`, `laptops`).
  - Carousel navigation: Prev / Next solution chain links all 12 lines cyclically.

### 2.2 Critical Defect Discoveries in Solutions:
- **`SOL-01` (Critical — Missing Arabic Solution Detail Page Template):**  
  The template file `apps/site/src/pages/ar/solutions/[id].astro` **does not exist**. Direct requests to Arabic solution detail URLs (e.g. `/ar/solutions/amc/`) return **HTTP 404 Not Found**.
- **`SOL-02` (Critical — Arabic Solution Cards are Static Non-Clickable Elements):**  
  In `apps/site/src/pages/ar/solutions/index.astro`, the 12 solution cards render plain `<h3>` and `<p>` tags without the `<a class="go2">` wrapper present in English. Arabic visitors cannot click to learn more about any solution line.
- **`SOL-03` (High — Solution Detail Language Switcher Demotion):**  
  On any English solution detail page (e.g. `/solutions/cloud-security/`), clicking the Arabic language switcher redirects to `/ar/solutions/` (the hub page) rather than preserving the specific solution context.

---

## 3. 📰 Vertical 3: Research, Insights & Editorial Newsroom Audit

The Insights desk publishes market intelligence, storage technical guides, and regulatory briefs (e.g., UAE Personal Data Protection Law).

### 3.1 13 Published Articles Audited (English & Arabic Parity)
All 13 articles were audited and verified active on both language channels:
1. `ai-in-erp-2026` — The Rise of AI in ERP Systems: Hype vs. Reality in 2026 (EN: 200, AR: 200)
2. `authorized-it-distributors-uae` — Why Sourcing from Authorized IT Distributors Matters in UAE (EN: 200, AR: 200)
3. `crucial-p510-launch` — Crucial P510 Gen5 NVMe SSD Launched in Dubai (EN: 200, AR: 200)
4. `cybersecurity-best-practices-uae` — Enterprise Cybersecurity Best Practices for UAE Businesses (EN: 200, AR: 200)
5. `ddr4-vs-ddr5` — DDR4 vs DDR5 RAM: Enterprise Performance and Upgrade Guide (EN: 200, AR: 200)
6. `enterprise-ssd-buying-guide-2026` — Enterprise SSD Buying Guide 2026: NVMe vs SATA (EN: 200, AR: 200)
7. `future-proof-it-infrastructure` — Future-Proofing IT Infrastructure: Scalability & Resilience (EN: 200, AR: 200)
8. `industry-4-manufacturing-erp` — Industry 4.0: Modern ERP for Textile & Garments Manufacturing (EN: 200, AR: 200)
9. `simal-two-decades` — Two Decades of IT Distribution in Dubai: The Simal Story (EN: 200, AR: 200)
10. `top-ssds-for-resellers-2025` — Top SSDs for Resellers in 2025: Channel Margins & Demand (EN: 200, AR: 200)
11. `uae-data-protection-pdpl` — UAE Personal Data Protection Law (PDPL): IT Compliance Checklist (EN: 200, AR: 200)
12. `unierp-transforms-sme` — How UniERP Transforms SME Operations in the GCC (EN: 200, AR: 200)
13. `why-gray-market-costs-more` — Why Gray-Market IT Hardware Costs More: Hidden Risks (EN: 200, AR: 200)

### 3.2 Gated 2026 Line Card Module (`insights-linecard`)
- Verified lead capture card on `/insights/` and `/ar/insights/`. Submitting an email triggers validation, downloads the line card PDF, and captures the lead.

### 3.3 Critical Defect Discoveries in Insights:
- **`INS-01` (High — One-Way Language Switcher Routing Trap):**  
  On any English insight article (e.g. `/insights/ai-in-erp-2026/`), clicking the Arabic language toggle in the utility strip navigates to `/ar/insights/` (the hub root), **even though the exact Arabic article exists and returns HTTP 200 OK** at `/ar/insights/ai-in-erp-2026/`. Conversely, switching from Arabic to English correctly maps to `/insights/ai-in-erp-2026/`.
- **`INS-02` (Medium — Sticky Table of Contents Viewport Collision):**  
  On viewports between 1060px and 1180px, the sticky sidebar navigation (`.art-toc`) overlaps the article content column due to fixed width calculations.
- **`INS-03` (Low — Missing Reading Time Metadata on Arabic Articles):**  
  English articles render reading time badges (e.g., `5 min read`), whereas Arabic templates omit reading duration indicators.

---

## 4. 🎛️ Portal CMS Studio Audit (Software, Solutions & Insights)

Tested the editorial controls inside CMS Studio (`/cms/`):
- **`#/software-suites`:** Manages UniERP feature modules, pricing toggles, and GCC compliance badges.
- **`#/it-solutions`:** Accurately lists all 12 IT solution lines. Clicking `Edit Line ➔` opens the structured visual inspector with Zero-JSON controls for headlines, scope bullets, and icon selection.
- **`#/amc-tiers`:** Allows visual editing of SLA response times across Bronze, Silver, and Gold tiers.
- **`#/insights`:** Allows creating, updating, and drafting articles with human-friendly inputs for Title, Category, Reading Time, Author, and Content Paragraphs.
- **`CMS-01` (Medium — IT Solutions Table Lacks Language Separation Toggle):**  
  In `#/it-solutions`, the solution table does not provide an EN/AR language toggle filter, displaying only English block records.

---

## 5. 📋 Master Defect Registry (Software, Solutions & Insights)

The table below itemizes all **12 verified defects** identified during today's audit:

| Bug ID | Vertical | Target Location | Severity | Defect Summary & Observed Impact | Recommended Engineering Fix |
| :--- | :--- | :--- | :---: | :--- | :--- |
| **SFT-01** | Software | `/ar/software/` | **Critical** | The entire interactive ROI calculator section is missing from the Arabic software hub. | Port ROI calculator HTML & JS from English to `apps/site/src/pages/ar/software/index.astro`. |
| **SFT-02** | Software | `/ar/software/*` | **Critical** | Arabic software subdirectories (`modules/`, `services/`, `products/`) do not exist (HTTP 404). | Build corresponding `.astro` route files in `apps/site/src/pages/ar/software/`. |
| **SOL-01** | Solutions | `/ar/solutions/[id]/` | **Critical** | Missing Arabic detail page template `ar/solutions/[id].astro`; direct URLs return HTTP 404. | Create `apps/site/src/pages/ar/solutions/[id].astro` mirroring English with RTL layout. |
| **SOL-02** | Solutions | `/ar/solutions/` | **Critical** | Arabic IT solution cards render as static text without `<a>` links to detail pages. | Add `<a class="go2" href={`/ar/solutions/${s.id}/`}>` to cards in `ar/solutions/index.astro`. |
| **SOL-03** | Solutions | `/solutions/[id]/` | **High** | Language switcher on solution detail page demotes user to `/ar/solutions/` hub instead of detail page. | Update `BaseLayout.astro` route parser to preserve solution ID: `/ar/solutions/${id}/`. |
| **INS-01** | Insights | `/insights/[slug]/` | **High** | Language switcher on English article redirects to `/ar/insights/` root instead of the matching Arabic article. | Update `BaseLayout.astro` to map `/insights/${slug}/` directly to `/ar/insights/${slug}/`. |
| **LNK-01** | Global | Utility Bar | **High** | Persistent Header "Sign in" link points to `/admin` returning HTTP 404 on all 6 hubs. | Update utility bar anchor in `BaseLayout.astro` from `/admin` to `/cms/` or login portal. |
| **SFT-03** | Software | `/software/#demo` | **Medium** | Demo request form message textarea allows raw unescaped HTML/script tags. | Add input sanitization and strict type validation in demo form submission handler. |
| **INS-02** | Insights | `/insights/[slug]/` | **Medium** | Sticky table of contents (`.art-toc`) collides with article body text between 1060px and 1180px. | Increase media query threshold from `min-width:1060px` to `min-width:1200px`. |
| **CMS-01** | CMS Studio | `#/it-solutions` | **Medium** | IT Solutions table in CMS lacks an EN/AR language toggle filter, displaying only English records. | Add language tab selector in `viewItSolutions()` to allow editing Arabic solution lines. |
| **INS-03** | Insights | `/ar/insights/` | **Low** | Arabic insight article cards omit estimated reading time badges shown on English counterpart. | Add reading time calculation badge to Arabic article cards. |
| **SFT-04** | Software | `/software/` | **Low** | ROI calculator manual process hours input lacks upper boundary clamping (allows >720 hrs/mo). | Add `max="720"` attribute to `#r-man` input. |

---

## 6. 🛠️ Actionable Remediation Roadmap

1. **P0 (Immediate Fix — Arabic Solutions & Software Parity):**
   - Create `apps/site/src/pages/ar/solutions/[id].astro` querying `it_solution` where `lang = 'ar'`.
   - Update `apps/site/src/pages/ar/solutions/index.astro` to wrap solution cards in clickable links pointing to `/ar/solutions/${s.id}/`.
   - Port the UniERP ROI calculator into `apps/site/src/pages/ar/software/index.astro` with Arabic copy and currency indicators.

2. **P1 (Immediate Fix — Language Switcher Twin Mapping):**
   - Update `BaseLayout.astro` (lines 220–225) to preserve entity slugs:
     - `/solutions/${id}/` ➔ `/ar/solutions/${id}/`
     - `/insights/${slug}/` ➔ `/ar/insights/${slug}/`
   - Eliminate the one-way routing trap on insight articles.

3. **P2 (CMS & Form Enhancements):**
   - Add language toggle in CMS `viewItSolutions()` to enable direct visual editing of Arabic solution copies.
   - Adjust CSS media queries for `.art-toc` on insight detail pages to prevent tablet viewport collisions.

---

**Report Authored By:** Tahmid Sadat  
**Repository Branch:** `sadat`  
**Execution Timestamp:** 11 November 2026, 18:18 GMT+6  
**Git Push Remote:** `https://github.com/TahmidSadat02/DailyWork.git`
