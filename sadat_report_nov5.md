# Daily Work Report — Tahmid Sadat

**Date:** 05 November 2026  
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

Today's operational QA testing on **05 November 2026** continued the structured adversarial, navigation integrity, and security auditing of the Simal corporate platform. 

Today's testing focused on three major objectives:
1. **Regression Retest of the 24 Issues from Nov 4**: Systematically re-evaluating each previously cataloged defect on the live staging server (`http://182.160.112.72:8000/`) to track resolution status across public routes, CMS hash views, and server handlers.
2. **Comprehensive Verification of Newly Deployed Resource Assets & Whitepapers**: Thoroughly auditing the newly seeded **13 Publication-Grade PDF Documents** and Press Room Downloads across `/resources/` and `/ar/resources/`, checking HTTP headers, MIME types, caching behaviors, and modal lead-capture triggers (`data-gate`).
3. **Phase 2 Advanced Adversarial & Network Security Probes**: Conducting specialized cross-origin resource sharing (CORS) reflection attacks, API rate-limiting stress testing, clickjacking / framing posture evaluations, and UAE/Saudi PDPL cookie consent audits.

### Summary Metrics of Today's Audit:
| Metric / Scope Category | Day 1 (Nov 4) | Day 2 (Nov 5) | Trend / Delta |
| :--- | :---: | :---: | :--- |
| **Public Live Routes Re-verified** | 253 | **253** | Complete twin coverage maintained |
| **Downloadable PDF Whitepapers Verified** | 0 | **13 Active (200 OK)** | 100% verified active on EN & AR |
| **Previous Nov 4 Defects Retested** | 24 | **24 Retested** | 24 Open (No backend patch deployed yet) |
| **New Security Vulnerabilities Discovered** | — | **3 New Defects** | 1 Critical (CORS reflection), 1 High, 1 Med |
| **Total Active Defect Registry** | 24 | **27 Defects** | Documented with reproduction vectors |
| **Code / Config / DB Modifications** | 0 | **Strict Zero (0)** | Read-Only QA Mandate strictly upheld |

---

## 1. 🔄 Regression Audit: Verification Status of Nov 4 Defects

All 24 defects documented in yesterday's report (`sadat_report_nov4.md`) were systematically retested against the live deployment on `http://182.160.112.72:8000/`.

### Critical & High Severity Retest Findings:
1. **`AUT-01` (Critical — Information Disclosure on `/api/settings`):**  
   - **Retest Result:** **STILL VULNERABLE (UNRESOLVED)** 🚨  
   - Direct `curl -s http://182.160.112.72:8000/api/settings` continues to respond with **HTTP 200 OK** without session credentials, leaking corporate TIN (`100123456700003`), Peppol ID (`0235:1001234567`), and internal KYC backend IP/port (`http://182.160.112.72:3100`).
2. **`AUT-02` (Critical — Unauthenticated CMS `#app-container` Exposure):**  
   - **Retest Result:** **STILL VULNERABLE (UNRESOLVED)** 🚨  
   - Direct incognito navigation to `/cms/#/users-management` continues to render `#app-container` in DOM with `display: grid` due to persistent selector mismatch (`$('#app')` vs `#app-container`).
3. **`AUT-03` (High — Session Cache Leak on Browser Back Button):**  
   - **Retest Result:** **STILL VULNERABLE (UNRESOLVED)**  
   - Logging out of CMS and clicking the browser's Back button reloads cached DOM containing authenticated admin views. Responses still lack `Cache-Control: no-store`.
4. **`LNK-01` (High — Header Utility "Sign In" Link 404):**  
   - **Retest Result:** **STILL BROKEN (UNRESOLVED)**  
   - Utility bar "Sign in" anchor points to `/admin` returning **HTTP 404 Not Found**.
5. **`LNK-02` (High — Contact Page Fast-Path Port 3100):**  
   - **Retest Result:** **STILL BROKEN (UNRESOLVED)**  
   - `/contact/` continues to link to `http://182.160.112.72:3100/admin`, failing to resolve on public networks.
6. **`LNK-03` (High — Hardcoded `localhost:8090` in Sitemaps):**  
   - **Retest Result:** **STILL BROKEN (UNRESOLVED)**  
   - HTML sitemaps on `/sitemap/` and `/ar/sitemap/` retain hardcoded `http://localhost:8090/`.
7. **`LNK-04` through `LNK-07` (Medium — Language Switcher Route Misdirection):**  
   - **Retest Result:** **STILL BROKEN (UNRESOLVED)**  
   - Language switchers on `/brands/`, `/careers/`, `/compare/`, and `/resources/` continue redirecting to `/ar/products/` or `/ar/about/` because `exactTwinRoots` array in `BaseLayout.astro` has not yet been updated.
8. **`BRK-01` & `BRK-02` (Medium — Horizontal Overflows at 375px/390px Viewports):**  
   - **Retest Result:** **STILL BROKEN (UNRESOLVED)**  
   - `/onboarding/` and `/ar/onboarding/` overflow viewport width by 38px and 44px on mobile screens.
9. **`BRK-03` & `BRK-04` (Medium/Low — Broken Hardware Catalog Product Images):**  
   - **Retest Result:** **STILL BROKEN (UNRESOLVED)**  
   - 58 product catalog image requests return HTTP 404 due to missing local static assets.

*Takeaway:* No production patch deployment occurred between Nov 4 and Nov 5 for these items; all 24 issues remain open and tracked.

---

## 2. 📑 Audit of Newly Deployed Features: Downloadable Resources & Whitepapers

Today's audit performed an in-depth operational verification of the newly merged resource library, case studies, and whitepaper downloads across `/resources/` and `/ar/resources/`.

### 2.1 Complete PDF Asset Availability & Header Verification
All 13 newly seeded publication-grade PDF documents were requested via automated script to verify HTTP status, Content-Type, Content-Length, and Cache-Control headers:

| # | Asset URL Path | HTTP Status | Content-Type | File Size | Cache-Control Header |
|---|---|:---:|---|---|---|
| **1** | `/assets/downloads/whitepaper-true-cost-of-grey-market-it.pdf` | **200 OK** ✅ | `application/pdf` | 79.0 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **2** | `/assets/downloads/whitepaper-erp-selection-for-smes-2026.pdf` | **200 OK** ✅ | `application/pdf` | 82.4 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **3** | `/assets/downloads/whitepaper-peppol-readiness-checklist-uae.pdf` | **200 OK** ✅ | `application/pdf` | 76.1 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **4** | `/assets/downloads/datasheet-accessories.pdf` | **200 OK** ✅ | `application/pdf` | 68.3 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **5** | `/assets/downloads/datasheet-gaming.pdf` | **200 OK** ✅ | `application/pdf` | 71.5 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **6** | `/assets/downloads/datasheet-internal-ssd.pdf` | **200 OK** ✅ | `application/pdf` | 74.8 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **7** | `/assets/downloads/datasheet-laptops.pdf` | **200 OK** ✅ | `application/pdf` | 69.9 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **8** | `/assets/downloads/datasheet-memory-ram.pdf` | **200 OK** ✅ | `application/pdf` | 73.2 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **9** | `/assets/downloads/datasheet-monitors.pdf` | **200 OK** ✅ | `application/pdf` | 70.4 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **10** | `/assets/downloads/datasheet-portable-ssd.pdf` | **200 OK** ✅ | `application/pdf` | 72.1 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **11** | `/assets/downloads/media-kit-brand-guidelines.pdf` | **200 OK** ✅ | `application/pdf` | 91.2 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **12** | `/assets/downloads/press-releases-corporate-factsheet.pdf` | **200 OK** ✅ | `application/pdf` | 84.7 KB | `public, max-age=86400, stale-while-revalidate=604800` |
| **13** | `/assets/downloads/product-catalog-datasheet-directory.pdf` | **200 OK** ✅ | `application/pdf` | 98.6 KB | `public, max-age=86400, stale-while-revalidate=604800` |

### 2.2 Bilingual Parity on Arabic Resources (`/ar/resources/`)
- **Arabic Twin Verification:** Executed route parsing on `/ar/resources/`. Confirmed exact **1:1 parity with 13 Arabic cards**, linking to the corresponding publication-grade PDFs with authentic Arabic titles and RTL reading layouts.
- **Lead Capture Modal Triggering:** Verified that Whitepaper cards contain `data-gate` and `gate-trigger` classes. Clicking triggers the enterprise lead capture modal before opening or downloading the document.

---

## 3. 🛡️ Phase 2 Advanced Adversarial & Security Probes (New Findings)

Today's adversarial testing executed network-level and API-level penetration probes targeting cross-origin boundaries, denial of service / brute-force resistance, and session security.

### 🚨 SEC-01 (Critical): Arbitrary CORS Origin Reflection & Wildcard Policy (CWE-942)
- **Vulnerability Discovered:**  
  1. Testing `GET /api/settings` with an arbitrary `Origin: https://attacker.com` header returns:
     ```http
     Access-Control-Allow-Origin: *
     ```
     This allows any client-side JavaScript on any website worldwide to read Simal internal infrastructure settings and tax registration numbers.
  2. Testing CORS Preflight (`OPTIONS /api/settings`) with `Origin: https://evil.example.com` returns:
     ```http
     HTTP/1.1 204 No Content
     Access-Control-Allow-Origin: https://evil.example.com
     Access-Control-Allow-Credentials: true
     Access-Control-Allow-Methods: GET, POST, PUT, DELETE, PATCH, OPTIONS
     Access-Control-Allow-Headers: Content-Type, X-CSRF-Token, Authorization, X-Request-Id
     ```
- **Adversarial Impact:**  
  The backend dynamically reflects *any* submitted `Origin` header and explicitly pairs it with `Access-Control-Allow-Credentials: true`. A malicious webpage visited by an authenticated Simal CMS administrator can issue cross-origin credentialed requests to read protected admin data and execute unauthorized CMS actions.
- **Severity:** **Critical (CVSS 8.8)**

### ⚠️ SEC-02 (High): Absence of API Rate Limiting & DoS Susceptibility (CWE-799)
- **Vulnerability Discovered:**  
  - Conducted high-frequency request bursts against `/api/settings`, `/api/contact`, and `/api/newsletter`.
  - Sent 50 consecutive requests within 2 seconds. All 50 requests returned **HTTP 200 OK** without triggering any `HTTP 429 Too Many Requests` or temporary client throttling.
- **Adversarial Impact:**  
  The absence of rate limiting on submission endpoints leaves `/api/contact` vulnerable to automated spam bot abuse and mailbox flooding, and exposes authentication endpoints to brute-force credential stuffing without rate-based lockouts.
- **Severity:** **High (CVSS 7.1)**

### ⚠️ SEC-03 (Medium): Cookie Consent & Session Scoping Under PDPL Compliance (CWE-614)
- **Vulnerability Discovered:**  
  - Tested the UAE PDPL / Saudi PDPL cookie consent banner (`#cookie-banner`).
  - While visual toggles exist for "Strictly Necessary", "Performance & Analytics", and "Marketing", inspecting client storage revealed that analytics scripts (`ga4-loader.js`) and tracking cookies are pre-loaded in document `<head>` before user interaction with `#cookie-accept-all`.
  - Stored cookies lack explicit `SameSite=Strict` attributes on session identifiers.
- **Severity:** **Medium (CVSS 5.3)**

---

## 4. ⚡ Performance & Core Web Vitals (CWV) Network Audit

Audited real-world network latency and Core Web Vitals performance on `http://182.160.112.72:8000/`:

| Route Evaluated | TTFB (Time to First Byte) | DOM Content Loaded | Total Transfer Size | LCP Indicator | Status |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Homepage (`/`)** | 42 ms | 310 ms | 128 KB | Hero Image / Text (0.9s) | **Good (Fast)** ✅ |
| **Arabic Home (`/ar/`)** | 45 ms | 318 ms | 134 KB | Hero Slide (0.95s) | **Good (Fast)** ✅ |
| **Products Catalog (`/products/`)** | 58 ms | 480 ms | 280 KB | SKU Grid (1.4s) | **Needs Image Fix** ⚠️ |
| **Resources Hub (`/resources/`)** | 41 ms | 340 ms | 155 KB | Whitepaper Grid (1.1s) | **Good (Fast)** ✅ |
| **Contact Desk (`/contact/`)** | 39 ms | 290 ms | 98 KB | Lead Card (0.8s) | **Good (Fast)** ✅ |
| **CMS Studio (`/cms/`)** | 48 ms | 520 ms | 310 KB | App Container (1.6s) | **Acceptable** ⚠️ |

---

## 5. 📊 Master Defect Progression Matrix (Nov 4 vs Nov 5)

The table below reconciles all findings across yesterday's baseline audit and today's Day 2 testing:

| Bug ID | Category | Target Location | Severity | Day 1 (Nov 4) | Day 2 (Nov 5) | Current Status & Notes |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **AUT-01** | Security / Auth | `/api/settings` | **Critical** | Discovered | Retested | **OPEN** — Unauthenticated PII/network leak persists. |
| **AUT-02** | Security / Auth | `/cms/#/users-management` | **Critical** | Discovered | Retested | **OPEN** — `#app-container` exposed via selector bug. |
| **SEC-01** | Security / CORS | `/api/*` & Preflight | **Critical** | — | **NEW** 🚨 | **OPEN** — Arbitrary Origin reflection + Credentials true. |
| **SEC-02** | Security / DoS | Public `/api/*` endpoints | **High** | — | **NEW** ⚠️ | **OPEN** — Zero rate limiting / throttling on bursts. |
| **SEC-03** | Security / Privacy | Cookie Consent / Storage | **Medium** | — | **NEW** ⚠️ | **OPEN** — Pre-consent analytics load under PDPL. |
| **AUT-03** | Security / Session | `/cms/` (Logout Gate) | **High** | Discovered | Retested | **OPEN** — Browser Back button cache leak persists. |
| **LNK-01** | Link Integrity | Global Header Topbar | **High** | Discovered | Retested | **OPEN** — "Sign in" points to `/admin` (404). |
| **LNK-02** | Link Integrity | `/contact/` Fast-Path | **High** | Discovered | Retested | **OPEN** — Port 3100 unreachable. |
| **LNK-03** | Link Integrity | `/sitemap/` & `/ar/sitemap/` | **High** | Discovered | Retested | **OPEN** — Hardcoded `localhost:8090` in sitemap. |
| **LNK-04** | Link Integrity | `/brands/` (Lang Switch) | **Medium** | Discovered | Retested | **OPEN** — Misroutes to `/ar/products/`. |
| **LNK-05** | Link Integrity | `/careers/` (Lang Switch) | **Medium** | Discovered | Retested | **OPEN** — Misroutes to `/ar/about/`. |
| **LNK-06** | Link Integrity | `/compare/` (Lang Switch) | **Medium** | Discovered | Retested | **OPEN** — Misroutes to `/ar/products/`. |
| **LNK-07** | Link Integrity | `/resources/` (Lang Switch) | **Medium** | Discovered | Retested | **OPEN** — Misroutes to `/ar/about/`. |
| **LNK-08** | SEO / Sitemap | `sitemap.xml` | **Medium** | Discovered | Retested | **OPEN** — Arabic static routes omitted from XML. |
| **LNK-09** | Link Integrity | `/software/modules/*` | **High** | Discovered | Retested | **OPEN** — Financial management module 404. |
| **LNK-10** | Link Integrity | `/software/services/*` | **High** | Discovered | Retested | **OPEN** — Custom software development 404. |
| **LNK-11** | Link Integrity | `/insights/*` | **High** | Discovered | Retested | **OPEN** — UAE PDPL guide 404. |
| **LNK-12** | Link Integrity | `/cms/login.html` | **High** | Discovered | Retested | **OPEN** — Private IP `172.17.15.133:3000` link. |
| **LNK-13** | Link Integrity | `/legal/terms-of-sale/` | **High** | Discovered | Retested | **OPEN** — Terms of sale 404 on EN and AR. |
| **FLD-01** | Form / Security | `/contact/` -> API | **Medium** | Discovered | Retested | **OPEN** — Unsanitized raw HTML accepted. |
| **AUT-04** | CMS Architecture | `#/system-telemetry` | **Medium** | Discovered | Retested | **OPEN** — Raw `<pre>` JSON violates Zero-JSON rule. |
| **AUT-05** | CMS Architecture | `#/revisions` | **Medium** | Discovered | Retested | **OPEN** — Raw JSON payload toggle present. |
| **CRD-01** | Architecture / SSG | `#/announcements` -> Live | **High** | Discovered | Retested | **OPEN** — No auto SSG rebuild on save. |
| **CRD-02** | Architecture / SSG | `#/job-posts` -> Live | **High** | Discovered | Retested | **OPEN** — Careers page requires manual rebuild. |
| **CRD-03** | CMS API | `#/settings` | **High** | Discovered | Retested | **OPEN** — `kycApiBase` edits silently revert. |
| **BRK-01** | Responsive UI | `/onboarding/` (375px) | **Medium** | Discovered | Retested | **OPEN** — 38px horizontal overflow. |
| **BRK-02** | Responsive UI | `/ar/onboarding/` (375px) | **Medium** | Discovered | Retested | **OPEN** — 44px horizontal overflow in RTL. |
| **BRK-03** | Media Integrity | `/products/` & `/ar/` | **Medium** | Discovered | Retested | **OPEN** — 58 missing SKU images (404). |
| **BRK-04** | Media Integrity | Homepage (`/` & `/ar/`) | **Low** | Discovered | Retested | **OPEN** — 4 missing homepage images (404). |

---

## 6. 🛠️ Prioritized Remediation Roadmap for Engineering Sprint

To resolve both the Nov 4 baseline issues and today's new security vulnerabilities, the engineering team should execute the following prioritized patches:

1. **P0 (Immediate Hotfix — Security):**
   - **Fix CORS Middleware:** Restrict `Access-Control-Allow-Origin` to authorized corporate domains (`https://simalme.com`, `https://kyc.simalme.com`, `http://182.160.112.72:8000`). Never dynamically reflect arbitrary request origins while setting `Allow-Credentials: true`.
   - **Enforce Auth on `/api/settings`:** Attach session authentication middleware to block unauthenticated requests with HTTP 401.
   - **Implement API Rate Limiter:** Add `express-rate-limit` (e.g., 30 requests/minute per IP) on all public API endpoints (`/api/contact`, `/api/newsletter`, `/api/settings`).
2. **P1 (High Priority — Navigation & Session):**
   - Fix Header topbar "Sign In" link to point to `/cms/` (or partner portal) instead of `/admin`.
   - Replace `http://localhost:8090/` with relative `/cms/` in HTML sitemaps.
   - Update `exactTwinRoots` array in `BaseLayout.astro` to include `brands`, `careers`, `compare`, and `resources`.
   - Add `Cache-Control: no-store` header on `/cms/` routes to prevent back-button cache exposure.
3. **P2 (Medium Priority — Responsive & Media):**
   - Fix 375px overflow on `/onboarding/` and `/ar/onboarding/` by setting `max-w-full overflow-x-hidden` and `flex-wrap` on step chips.
   - Reconcile database image paths in `products.images` with files in `public/assets/img/products/` or provide a default fallback SVG image.

---

**Report Authored By:** Tahmid Sadat  
**Repository Branch:** `sadat`  
**Execution Timestamp:** 05 November 2026, 18:38 GMT+6  
**Git Push Remote:** `https://github.com/TahmidSadat02/DailyWork.git`
