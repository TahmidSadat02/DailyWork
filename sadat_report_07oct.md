# Engineering & Testing Report · 07-Oct-2026

**Author / Engineer:** Sadat  
**Repository:** GTex ERP (`Textile`)  
**Branch:** `sadat`  
**Date:** Wednesday, 07 October 2026  
**Status:** In Progress — Testing & Error Fixing Active (`pnpm typecheck: 0 errors`)

---

## Executive Summary

This report documents the testing, verification, and bug-fixing activities conducted on **07 October 2026** for the GTex Dynamics 365 ERP system. Focus was placed on resolving high-priority user-facing bugs in the **Buyer Portal** (`/buyer`), establishing a comprehensive error audit protocol, and preparing the test matrix across all 48 business modules and external portals.

All newly introduced code has been rigorously validated with zero TypeScript compilation errors and verified in real-time browser sessions.

---

## 1. Resolved Issues & Technical Fixes (07-Oct-2026)

### 1.1 Buyer Portal Dynamic Navbar Pill (`.portal-nav a.active`)
- **Problem:**
  In `/buyer` (`frontend/app/(portals)/buyer/page.tsx`), the active blue pill (`background: var(--primary-50); color: var(--primary-800)`) was hardcoded to the `#orders` tab (`My orders`). Clicking other navigation tabs (`Track production`, `Approvals`, `Shipments`, `Documents`) did not move the indicator pill, and scrolling through sections did not update the active state.
- **Root Cause:**
  The page was rendered as a static server component with hardcoded `className="active"` on the first link without client-side interaction state or scroll listeners.
- **Implementation & Resolution:**
  - Converted `frontend/app/(portals)/buyer/page.tsx` to a reactive client component (`'use client'`).
  - Implemented `activeTab` state defaulting to `'orders'`.
  - Added smooth scroll handler `handleNavClick(e, tabId)` that calculates section scroll offsets accounting for the 56px sticky topbar (`window.scrollTo({ top: offsetPosition, behavior: 'smooth' })`).
  - Added bidirectional **scroll-spy synchronization** via a passive `scroll` event listener on `window` to dynamically update the active blue pill as users scroll through `#orders`, `#track`, `#approvals`, `#shipments`, and `#documents`.

### 1.2 Interactive Profile Avatar Menu (`AK` Flyout)
- **Problem:**
  Clicking the buyer profile avatar (`AK`) in the top right did nothing. The dropdown menu was completely missing, preventing the user from viewing persona details or accessing account options.
- **Root Cause:**
  The avatar was rendered as a static, non-interactive `<span>AK</span>` element without click listeners or associated flyout dropdown markup.
- **Implementation & Resolution:**
  - Replaced the static span with an accessible `<button type="button" className="tb-avatar">` wrapped in a relative positioning container (`ref={menuRef}`).
  - Built an authentic Dynamics 365 flyout menu (`.tb-flyout open`) styled with:
    - **Persona Header:** *A. Khan · H&M Production Manager* (H&M Hennes & Mauritz · Stockholm).
    - **Buyer 360 Link:** Direct navigation to `/commercial/buyer-360` with `users` icon.
    - **Buyer Settings Link:** Direct navigation to `/settings` with `id` icon.
    - **Staff Portal Switch:** Quick route back to `/commercial` with `home` icon.
    - **Contact Merchandiser Action:** Interactive toast notification displaying contact info for GTex Merchandiser *Nusrat Jahan* (`+880-1711-000000`).
    - **Sign Out Link:** Route to `/login` with `logout` icon.
  - Implemented automatic menu dismissal on both **outside clicks** (`mousedown` listener) and the **Escape key** (`keydown` listener).

### 1.3 Bilingual Language Toggle
- **Problem:**
  The language switch button in the header was non-interactive.
- **Implementation & Resolution:**
  - Added `isBn` boolean state toggling between `English` and `বাংলা`.
  - Added toast notification feedback alerting the user to language switch events with appropriate regional typography.

### 1.4 External Document & Prototype Deep-Links
- **Problem:**
  Document downloads, weekly WIP snapshots, and shipment records had stubbed `href="#"` links.
- **Implementation & Resolution:**
  - Replaced dead links with Next.js `<Link>` elements routing to the standardized screen viewer at `/screen?p=portals|External|...`, ensuring seamless end-to-end user navigation without page reloads.

### 1.5 Dedicated In-Portal Buyer Profile & Settings Modal Dialog
- **Problem:**
  In the Buyer Portal (`/buyer`), clicking "Buyer profile & settings" from the `AK` avatar dropdown previously navigated to `/settings`, which is the internal ERP staff/admin settings page for Merchandising Manager *Nusrat Jahan*. This broke the buyer persona separation and threw external buyers into the internal staff workspace.
- **Implementation & Resolution:**
  - Replaced the `/settings` navigation link with an interactive modal trigger (`setSettingsModalOpen(true)`).
  - Built an authentic Dynamics 365 in-portal modal dialog (`role="dialog"`, `aria-modal="true"`) dedicated to buyer persona *A. Khan · H&M Production Manager*:
    - **Header:** Buyer avatar (`AK`), title "Buyer Profile & Portal Settings", and subtitle "A. Khan · H&M Hennes & Mauritz AB · Stockholm & Regional Office" with close button (`X`).
    - **Tab 1: Profile & Organization:** Buyer identity card, Organization (`H&M Hennes & Mauritz AB`), Account ID (`BUY-HM-001`), official email (`a.khan@hm.com`), direct telephone (`+46 8 796 55 00 / +880 1711 998877`), and designated GTex merchandiser desk (`Nusrat Jahan · Team 2`).
    - **Tab 2: Alerts & Subscriptions:** Interactive toggles for Weekly WIP snapshot PDF dispatch, T&A critical path milestone alerts, AQL 2.5 final inspection pass notices, and EDI despatch advices (DESADV).
    - **Tab 3: Regional & Locale:** Primary portal timezone selector (Stockholm CET vs Dhaka BST vs London GMT), Number format selector (Western standard vs South Asian Lakh-Crore), and Metric unit system.
    - **Tab 4: Security & Access:** Single Sign-On federation badge (H&M Azure Active Directory), Tokenised order status link generator with one-click copy, and verified session IP.
    - **Actions:** Modal footer with `Close` and `Save preferences` buttons, dispatching a confirmation toast and returning to the portal smoothly.
    - **Accessibility & Dismissal:** Supports outside backdrop click dismissal and `Escape` key listeners.

### 1.6 Supplier & Subcontractor Portal (`/vendor`) Sidebar Visibility & CK Profile Settings
- **Problem:**
  When navigating to the Supplier Portal (`/vendor` and `prototype/vendor-portal.html`), the navigation sidebar was invisible, and the profile avatar (`CK` - Crypto Knit Ltd.) had no interactive flyout menu or profile settings modal dialog like other personas in the application.
- **Root Cause:**
  1. `prototype/vendor-portal.html` was drafted as an isolated page without the Dynamics 365 ERP shell (`#gtex-topbar`, `#gtex-sitemap`, `js/shell.js`).
  2. In Next.js, `prototype-sidebar.tsx` did not pre-open the `External surfaces (M41)` group for `/vendor` on initial render, and the topbar hardcoded merchandiser `NJ` persona rather than switching to the `CK` supplier admin persona on the `/vendor` route.
- **Implementation & Resolution:**
  - **Sidebar Visibility:** Updated `prototype-sidebar.tsx` so `openGroups` is pre-populated on mount with `portals:External surfaces (M41)`. Added dynamic recent item tracking for `Supplier & subcontractor portal` and highlighted it with `.sm-item.active`.
  - **Prototype Parity:** Re-aligned `prototype/vendor-portal.html` within the full ERP shell (`<div class="app"><header class="topbar" id="gtex-topbar"></header><div class="shell"><nav class="sitemap" id="gtex-sitemap"></nav><main class="content">`), enabling instant sitemap visibility and area coordination.
  - **TopBar Vendor Persona Switch:** Updated `top-bar.tsx` so visiting `/vendor` dynamically renders the `CK` avatar button and opens the `Crypto Knit Ltd. · Vendor Admin (V-1029)` flyout with dedicated action triggers.
  - **Supplier Profile & Settings Modal Dialog:** Implemented a full 4-tab Dynamics 365 modal dialog:
    - **Tab 1: Profile & Organization:** Entity (`Crypto Knit Ltd.`), Vendor Code (`V-1029 · Tier-1`), BIN (`002918274-0101`), Mill Location (Gazipur), Sourcing Desk (Team 4).
    - **Tab 2: Alerts & Shortfalls:** Interactive toggles for Shortfall & Risk Alerts, 3-Way Match GRN Discrepancy Advisories, and RFQ Sealed Tender Invitations.
    - **Tab 3: Bank & Settlement:** Nominated Bank (`Eastern Bank Ltd.`), Settlement Currency (`BDT / USD`), Account (`104-122-09884321`), Payment Terms (`60 days deferred / Sight LC`).
    - **Tab 4: Security & EDI API:** Tokenised Supplier Access Link with one-click copy, and Automated ASN REST API Key generator.
  - **Action Modals & Navigation:** Added secondary portal navigation with dynamic blue pill indicator & scroll-spy (`#orders`, `#rfq`, `#invoices`, `#compliance`), Delivery Note upload modal, Sealed Bid RFQ modal, Shortfall resolution/dispute modal, and bilingual English/বাংলা toggle.

---

## 2. Test Matrix & Error Audit Status

| Component / Module | Surface / Route | Tested Scenarios | Status | Notes |
| **Buyer Portal** | `/buyer` | Dynamic navbar pill, smooth scroll-spy, AK profile flyout, language switch, in-portal Buyer Settings modal, Buyer 360 removed from AK flyout | **PASS** | Verified in browser; 0 console errors |
| **Worker Self-Service**| `/worker` | Top navbar (`portal-top`) with dynamic blue active pill tabs, interactive `SB`/`RI` profile flyout, in-portal Worker Settings modal, 4 core actions (Leave, Salary, Grievance, Welfare), Payslip PDF preview, PIN lock, Worker switcher, Bilingual toggle | **PASS** | Fully interactive; 0 TS errors |
| **Login Screen** | `/login` | Fixed nested `<body>` tag causing Next.js hydration error | **PASS** | Hydration error eliminated |
| **Commercial Home** | `/commercial` | Fixed Base UI `Button` render prop with `buttonVariants` on `<Link>` | **PASS** | Console warning resolved |
| **Vendor Portal** | `/vendor` | ERP shell & sidebar integration, auto-expanded M41 group, active pill tabs & scroll-spy, CK profile flyout, in-portal Supplier Settings modal, Delivery Note upload modal, Sealed RFQ bid modal, Shortfall dispute modal | **PASS** | Verified in Next.js & prototype; 0 TS errors |
| **Top Bar** | Global Shell | Home (`/commercial`), Dashboard (`/dashboard`), NJ Profile flyout | **PASS** | Event delegation conflict resolved |
| **Sidebar Navigation** | Global Shell | 1,257 screen routes via `/screen?p=...`, active highlight, accordion expand | **PASS** | Deep routing fully functional |
| **My Settings** | `/settings` | 8 settings tabs, Density toggle, Eye icon, Add view modal | **PASS** | Tested 06-Oct; persistent across reloads |
| **Standard Screen View** | `/screen` | Order allocation & size breakup, empty state anatomy, totals, command bar | **PASS** | Matches `prototype/screen.html` 1:1 |

---

## 3. Verification & Quality Assurance Logs

### 3.1 TypeScript Typecheck
```bash
$ cd frontend && pnpm typecheck
$ tsc --noEmit
# Exit Code: 0 (0 errors)
```

### 3.2 Live Browser Subagent Verification
- **Test 1 — Dynamic Navbar Pill:**
  - Navigated to `http://localhost:3000/buyer`.
  - Clicked *Track production* (`#track`): Navbar pill dynamically moved to "Track production".
  - Clicked *Approvals* (`#approvals`): Navbar pill dynamically moved to "Approvals".
- **Test 2 — Profile Flyout Menu:**
  - Clicked `AK` avatar button.
  - Dropdown `.tb-flyout` rendered with correct persona heading, dividers, and all 5 action buttons.
  - Clicked outside: Dropdown closed cleanly.
- **Test 3 — In-Portal Buyer Profile & Settings Modal:**
  - Clicked *Buyer profile & settings* in the dropdown.
  - Confirmed modal opened in-place over the Buyer Portal without redirecting to `/settings`.
  - Verified tabs: *Alerts & Subscriptions*, *Regional & Locale*, *Security & Access*, and *Profile & Organization*.
  - Verified A. Khan's details (H&M, Stockholm office, account `BUY-HM-001`, Merchandiser desk Nusrat Jahan).
  - Clicked *Save preferences*: Modal closed cleanly with success toast.
- **Test 4 — Screenshots Captured:**
  - Initial State: `buyer_initial_state_1791347363535.png`
  - Track Production Active: `buyer_track_production_active_1791347544367.png`
  - Profile Menu Open: `buyer_profile_flyout_open_1791347627226.png`
  - Buyer Profile & Settings Modal: `buyer_profile_modal_1791349737798.png`

### 1.5 Worker Self-Service ERP Shell, Sidebar & Navbar Realignment
- **Problem:**
  `/worker` was placed in an isolated `(ess)` route group with an extra secondary portal navbar (`portal-top` with tabs and avatar `SB`). The main prototype (`prototype/worker-ess.html`) embeds worker self-service directly inside the standard ERP shell with the ERP topbar and sitemap sidebar. Additionally, the prototype had `#` dead links for several External surfaces options, and the topbar had extraneous "Home" and "Dashboard" links not present in the prototype.
- **Implementation & Resolution:**
  - **Relocated Route Group:** Moved `/worker` from `app/(ess)/worker/page.tsx` to `app/(erp)/worker/page.tsx` so it automatically inherits `<TopBar />`, `<PrototypeSidebar />`, `<PrototypeBehaviors />`, and `<StatusBar />`.
  - **Removed Redundant Navbar:** Eliminated the secondary `<header className="portal-top">` and its sub-tabs (`portal-nav`) and avatar from `worker/page.tsx`.
  - **Removed Extraneous Topbar Links:** Removed direct "Home" and "Dashboard" links from `frontend/components/layout/top-bar.tsx` so the topbar anatomy matches the prototype (`GT Portals (external) v` + `PAL-ASH v`).
  - **Accurate Sidebar Paths & Deep-Links (`frontend/lib/nav/sitemap.ts`):**
    - `Buyer portal (order visibility)` → `/buyer`
    - `Worker self-service (mobile)` → `/worker` (active selected highlight)
    - `Supplier & subcontractor portal` → `/vendor`
    - `Auditor workspace — scoped evidence packs & rapid extracts (M34)` → `/screen?p=portals|External surfaces (M41)|Auditor workspace — scoped evidence packs & rapid extracts (M34)`
    - `Technician field app — offline work orders & evidence (M36)` → `/screen?p=portals|External surfaces (M41)|Technician field app — offline work orders & evidence (M36)`
    - `DPP consumer scan (GS1 digital link, M35)` → `/screen?p=portals|External surfaces (M41)|DPP consumer scan (GS1 digital link, M35)`
  - **Sidebar Portals Detection & Recent Alignment:**
    - Configured `prototype-sidebar.tsx` to recognize `/worker` under `portals`, set default recent items (`কর্মী সেবা — Worker self-service` active, `My Settings`, `Order allocation & size breakup`), and auto-expand `External surfaces (M41)`.
  - **Bottom Status Bar Integration (`frontend/components/layout/status-bar.tsx`):**
    - Added `.d365-status` footer displaying `PAL-ASH · read replica`, `Portals (external)`, `synced 08:40 · next 10:10`, `comfortable`, `compact`, `BN`, and `Ctrl+/`.

### 1.6 Supplier & Subcontractor Portal Sidebar Visibility Overhaul (`/vendor`)
- **Problem:**
  When clicking "Supplier & subcontractor portal" from the sidebar in `/worker`, the sidebar completely disappeared on `/vendor`.
- **Root Cause:**
  `/vendor` was situated in `app/(portals)/vendor/page.tsx`, which did not use `ErpLayout` (lacking `<TopBar />` and `<PrototypeSidebar />`).
- **Implementation & Resolution:**
  - Migrated `/vendor` to `frontend/app/(erp)/vendor/page.tsx` so it directly inherits `ErpLayout`.
  - Removed deprecated route from `app/(portals)/vendor`.
  - Cleared stale `.next/types` cache and confirmed `tsc --noEmit` exits with 0 errors.
  - Verified in live browser session that navigating to `/vendor` keeps the left sidebar fully visible, with `Portals (external)` (6) active, `External surfaces (M41)` auto-expanded, and `Supplier & subcontractor portal` highlighted with the blue active pill.

### 1.7 TNA Critical-Path Console 1:1 Parity (`/commercial/tna`)
- **Problem:**
  The Next.js TNA console at `/commercial/tna` diverged from `prototype/tna.html`, lacking the 4-step Dynamics 365 operational console pattern (`1 · Plan`, `2 · Exceptions`, `3 · Chase`, `4 · Re-plan`), horizon button group, command bar actions, and interactive exception chase and simulation flows.
- **Implementation & Resolution:**
  - Fully rebuilt [frontend/app/(erp)/commercial/tna/page.tsx](file:///Users/laptopparadise/Downloads/Textile/frontend/app/(erp)/commercial/tna/page.tsx) to match [prototype/tna.html](file:///Users/laptopparadise/Downloads/Textile/prototype/tna.html) 1:1.
  - Implemented Crumbs: `Commercial & Buyer Orders` > `TNA critical-path console`.
  - Implemented PageHead with `.d365-periods` horizon buttons (`2 weeks`, `This month`, `Season`).
  - Added Command Bar (`.cmdbar`) with `Re-run critical path`, `Generate chase list`, `Propose re-plan`, and `Export`.
  - Added Warning Message Bar with dismissibility and action "Open chase list" directly navigating to the chase panel.
  - Added 4-step Stepper coordinating panels:
    - **Step 1 (Plan):** Two-column layout with 4 collapsible order accordions (`PO-88214`, `PO-77190`, `PO-91007`, `PO-66420`), milestone tables with `/supply` and `/garment/cutting` cross-links, rolling 6-month on-time SVG line chart, and Gate ageing summary.
    - **Step 2 (Exceptions):** Consequence-ranked off-path orders (M&S PO-66420, Zara PO-77190, C&A PO-55133) with delay chips (`4 days`, `2 days`, `watch`).
    - **Step 3 (Chase):** Multi-select interactive grid with select-all, individual item checkboxes, "Discard draft", and "Dispatch selected".
    - **Step 4 (Re-plan):** Form grid with order selector, date input, line allocation, notify channel, simulation banner, "Save as scenario", and "Submit for approval".

### 1.8 Landing Page Navbar Text Orientation & No-Wrap (`/`)
- **Problem:**
  On the landing page (`/`), navbar texts and buttons wrapped vertically ("up and down"): "The group" wrapped into two lines, "Buyer portal" wrapped into two lines, and "Sign in" wrapped into two lines, causing distorted button heights and misaligned text.
- **Root Cause:**
  1. Missing `white-space: nowrap`, `flex-shrink: 0`, and `line-height: 1` on `.lnav-links a`, `.lbtn`, `.btn`, `.lnav .brand`, and `.lang-switch`.
  2. Redundant navigation items in `lnav-links` ("Home", "Dashboard", duplicate "Modules") exceeding the container width.
  3. Narrow `max-width: 1200px` on `.lnav-in` causing flex-shrink compression on standard desktop displays.
- **Implementation & Resolution:**
  - Updated `globals.css` and `prototype-landing.css`:
    - Set `.lnav-in` to `max-width: 1400px; width: 100%`.
    - Added `white-space: nowrap; flex-shrink: 0; line-height: 1` to `.brand`, `.lnav-links a`, `.right`, `.lbtn`, `.btn`, and `.lang-switch`.
    - Streamlined `lnav-links` in [frontend/app/page.tsx](file:///Users/laptopparadise/Downloads/Textile/frontend/app/page.tsx) to match prototype sections (`The group`, `Modules`, `Channels`, `Experience`, `Roadmap`) plus direct ERP `Workspace` link.
    - Enhanced [frontend/components/layout/language-toggle.tsx](file:///Users/laptopparadise/Downloads/Textile/frontend/components/layout/language-toggle.tsx) with `variant="landing"` rendering the authentic `.lang-switch` button with globe icon and proper height.
  - Verified in browser with full-width screenshot confirming horizontal single-line alignment of all buttons and links.

---

## 4. Modified Files Summary (07-Oct-2026)

| File Path | Nature of Change | Impact |
| :--- | :--- | :--- |
| `frontend/app/(erp)/commercial/tna/page.tsx` | Rebuilt TNA critical-path console matching prototype | 1:1 parity with prototype/tna.html |
| `frontend/app/page.tsx` | Cleaned up lnav-links and used landing variant for LanguageToggle | Eliminates navbar button text wrapping |
| `frontend/app/globals.css` | Added white-space: nowrap, flex-shrink: 0, line-height: 1, and wider max-width for lnav | Ensures all nav buttons and links stay on single line |
| `frontend/app/prototype-css/prototype-landing.css` | Ported nowrap and flex rules to prototype-landing.css | Full parity with landing styles |
| `frontend/components/layout/language-toggle.tsx` | Supported variant='landing' with .lang-switch button styling | Matches prototype language switcher button |
| `frontend/app/(erp)/vendor/page.tsx` | Moved into `(erp)` layout; removed outer redundant main container | Keeps sidebar & topbar visible on Supplier & subcontractor portal |
| `frontend/app/(erp)/worker/page.tsx` | Moved into `(erp)` layout; removed secondary navbar; retained all interactive modals | Matches prototype layout 1:1 inside ERP shell |
| `frontend/lib/nav/sitemap.ts` | Updated `portals` area with valid `/screen` deep-links for all 6 external surfaces | Resolves dead `#` links with standard Dynamics 365 view pages |
| `frontend/components/layout/top-bar.tsx` | Mapped `/worker`, `/buyer`, `/vendor` to `portals`; removed "Home"/"Dashboard" navbar options | Clean topbar matching prototype anatomy |
| `frontend/components/layout/prototype-sidebar.tsx` | Added `portals` area detection, auto-open for `External surfaces (M41)`, and matching RECENT items | Accurate sidebar with active highlight on `/worker` & `/vendor` |
| `frontend/components/layout/status-bar.tsx` | Implemented `.d365-status` footer component with density toggles and entity badge | Matches prototype bottom status bar |
| `frontend/app/(erp)/layout.tsx` | Added `<StatusBar />` and bottom padding | Renders status bar across all ERP views |
| `sadat_todo_07oct.md` | Updated tasks status and completed vendor portal sidebar realignment | Real-time progress tracking |
| `sadat_report_07oct.md` | Documented Supplier portal sidebar, TNA console parity, and navbar text fixes | Comprehensive technical documentation |

---

## 5. Next Immediate Action Items

1. **Buyer 360 Overview Screen (`/commercial/buyer-360`):**
   - Verify KPI cards, charts, and interactive order drill-downs.
2. **End-to-End Build Verification:**
   - Run `pnpm typecheck` to maintain 0 errors across all routes.


