# Testing & Error-Fixing Todo List · 07-Oct-2026

**Engineer:** Sadat  
**Repository:** GTex ERP (`Textile`)  
**Branch:** `sadat`  
**Date:** Wednesday, 07 October 2026  
**Goal:** Comprehensive testing, bug-fixing, and prototype-compliance validation across ERP modules, portals, navigation, and state workflows.

---

## Priority Legend
- **[P0 - Critical]**: Blocks user interaction, broken navigation, missing core functionality, or compilation/runtime errors.
- **[P1 - High]**: Design system mismatch, unstyled elements, non-dynamic states, or broken links.
- **[P2 - Medium]**: Edge-case validation, polish, animations, and non-blocking responsive layout improvements.

---

## 1. Portals & External Surfaces (Buyer & Vendor)

### 1.1 Buyer Portal (`/buyer`)
- [x] **[P0] Dynamic Navbar Active Indicator**: Blue pill (`.portal-nav a.active`) moves dynamically when clicking tabs (`My orders`, `Track production`, `Approvals`, `Shipments`, `Documents`).
- [x] **[P0] Scroll-Spy Synchronization**: Active navbar pill updates smoothly as the user scrolls down through the sections (`#orders`, `#track`, `#approvals`, `#shipments`, `#documents`).
- [x] **[P0] Profile Flyout Menu (`AK`)**: Clicking avatar opens interactive menu showing user persona (`A. Khan · H&M Production Manager`), `Buyer 360 overview`, `Buyer profile & settings`, `Back to GTex ERP`, `Contact merchandiser (Nusrat)`, and `Sign out`.
- [x] **[P0] Buyer Profile & Settings In-Portal Modal**: Clicking "Buyer profile & settings" opens an authentic in-portal modal for A. Khan (H&M Production Manager) with 4 tabs (Profile & Organization, Alerts & Subscriptions, Regional & Locale, Security & Access) instead of redirecting to the staff/admin settings.
- [x] **[P1] Outside-Click & Escape Dismissal**: Clicking outside the flyout/modal or hitting `Escape` cleanly dismisses both.
- [x] **[P1] Bilingual Switch (`English` / `বাংলা`)**: Interactive language button toggles active language indicator and toast feedback.
- [ ] **[P1] External Document Links**: Verify download/view links for certificates, lab reports, packing lists route to the proper preview modals or `/screen?p=...`.
- [ ] **[P2] Mobile / Responsive Test**: Check navbar wrapping and table overflowing on tablet and mobile viewports (< 768px).

### 1.2 Vendor / Supplier Portal (`/vendor`)
- [x] **[P0] Align Layout with Prototype (`prototype/vendor-portal.html`)**: Aligned Vendor Portal with `.portal-top`, `.portal-nav` (`Orders & deliveries`, `RFQs`, `Invoices & payments`, `Compliance`), and interactive profile flyout (`CK` - Crypto Knit Ltd.).
- [x] **[P0] ERP Shell & Sidebar Visibility**: Embedded supplier portal in full ERP shell so the sitemap sidebar is visible, auto-expanded to `External surfaces (M41)`, with `Supplier & subcontractor portal` highlighted as active.
- [x] **[P0] Supplier Profile & Settings Modal Dialog**: Built authentic 4-tab Dynamics 365 modal for Crypto Knit Ltd. (`Profile & Organization`, `Alerts & Shortfalls`, `Bank & Settlement`, `Security & EDI API`) triggered from `CK` avatar flyout in both TopBar and in-page portal header.
- [x] **[P1] Tab Navigation & Scroll-Spy**: Dynamic active pill with smooth scroll-spy navigation across `#orders`, `#rfq`, `#invoices`, `#compliance`.
- [x] **[P1] Action Modals / Upload Flows**: Implemented Delivery Note upload modal, RFQ sealed bid submission modal, shortfall resolution/dispute modal, and bilingual English/বাংলা toggle.
- [x] **[P2] KPI Cards & Alerts**: Rendered scorecard grades (`A−`), 4-pt QC metrics (`26 pts`), OTIF chips (`94%`), and shortfall callouts (`FB-2302`).

### 1.3 Worker Self-Service Portal (`/worker`)
- [x] **[P0] Realignment with ERP Shell (`app/(erp)/worker/page.tsx`)**: Removed isolated `(ess)` route group and placed `/worker` directly inside full ERP shell (`<TopBar />`, `<PrototypeSidebar />`, `<PrototypeBehaviors />`, and `<StatusBar />`).
- [x] **[P0] Removal of Secondary Navbar**: Removed extraneous `.portal-top` and `.portal-nav` tabs and profile avatar from the worker page, matching the main prototype anatomy (`prototype/worker-ess.html`).
- [x] **[P0] Accurate Sidebar Paths for External Surfaces (M41)**:
  - Buyer portal: `/buyer`
  - Worker self-service: `/worker` (active selected indicator)
  - Supplier & subcontractor portal: `/vendor`
  - Auditor workspace: `/screen?p=portals|External surfaces (M41)|Auditor workspace — scoped evidence packs & rapid extracts (M34)`
  - Technician field app: `/screen?p=portals|External surfaces (M41)|Technician field app — offline work orders & evidence (M36)`
  - DPP consumer scan: `/screen?p=portals|External surfaces (M41)|DPP consumer scan (GS1 digital link, M35)`
- [x] **[P0] Sidebar Active State & RECENT Items**: `Portals (external)` header (counter 6), RECENT items (`কর্মী সেবা — Worker self-service` active, `My Settings`, `Order allocation & size breakup`), and auto-expanded `External surfaces (M41)` group.
- [x] **[P1] Interactive Core Features**: Leave Application modal, Salary History modal, Grievance Box modal, Welfare Services modal, Payslip PDF preview modal, and Shared Kiosk PIN lock.

---

## 2. ERP Shell, Topbar & Global Navigation

### 2.1 Topbar Navigation (`top-bar.tsx`)
- [x] **[P0] Topbar Prototype Anatomy**: Removed extra "Home" and "Dashboard" links so topbar matches the prototype (`GT Portals (external) v` + `PAL-ASH v`).
- [x] **[P0] Landing Page Navbar Text Orientation & No-Wrap**: Fixed button texts wrapping vertically ("up and down") on landing page (`/`). Added `white-space: nowrap`, `flex-shrink: 0`, and `line-height: 1` to `.lnav-in`, `.brand`, `.lnav-links a`, `.right`, `.lbtn`, `.btn`, and `.lang-switch`. Streamlined navigation links to match prototype sections and styled language switcher.
- [x] **[P0] TNA Critical-Path Console Parity (`/commercial/tna`)**: Rebuilt `/commercial/tna` to match `prototype/tna.html` with D365 Crumbs, Horizon buttons (`2 weeks`, `This month`, `Season`), Command Bar (`Re-run`, `Generate chase list`, `Propose re-plan`, `Export`), Warning Message Bar with direct action to chase list, 4-step interactive Stepper (`Plan`, `Exceptions`, `Chase`, `Re-plan`), collapsible order details, rolling 6-month on-time SVG line chart, Gate ageing summary, ranked exceptions with consequence chips, multi-select chase list, and simulation re-plan form.
- [x] **[P0] Staff Profile Flyout (`NJ`)**: Avatar click toggles `.tb-flyout` without closing prematurely from outside-click listener conflicts.
- [ ] **[P1] Company Selector (`G-DHK`)**: Test switching between legal entities (GTex Dhaka, GTex Chittagong, GTex London Commercial).
- [ ] **[P1] Global Search (`Cmd+K` / Search bar)**: Test search modal, quick screen filtering, and keyboard navigation.
- [ ] **[P1] Notification Center (`bell` icon)**: Test notification badge, flyout drawer, and mark-as-read interactions.
- [ ] **[P2] Mega Menu (`Waffle` icon / Modules dropdown)**: Verify all 48 modules open smoothly with direct deep-links.

### 2.2 Sidebar Sitemap (`prototype-sidebar.tsx`)
- [x] **[P0] Deep Screen Routing**: Clicking any catalogue sub-module routes to `/screen?p=area|group|screen`.
- [x] **[P0] Active Screen Highlighting**: URL parameter matches active menu item and highlights it with the blue pill.
- [x] **[P0] Group Auto-Expansion**: Parent accordion group auto-expands when navigating directly to a sub-module.
- [ ] **[P1] Collapse / Expand Sidebar**: Test compact rail toggle (`Ctrl+[` or toggle button) preserving state.
- [ ] **[P1] Pinned / Favorites Section**: Verify pinning screens to "Recent" or "Favorites" persists in session.

---

## 3. Core Business Modules & Workflows

### 3.1 Commercial & Merchandising (`/commercial`, `/commercial/buyer-360`)
- [ ] **[P0] Buyer 360 Dashboard**: Verify H&M, Inditex, Marks & Spencer buyer performance tiles, seasonal order books, and WIP velocity metrics.
- [x] **[P0] Order Allocation & Size Breakup (`/screen?p=commercial|Order book (M02)|Order allocation & size breakup`)**: Verify standard view anatomy matching `prototype/screen.html` (header, command bar, message bar, grid, empty state, totals).
- [ ] **[P1] Costing & Pre-Costing Sheets**: Test BOM calculations, SMV rates, fabric consumption inputs, and CM margin indicators.
- [ ] **[P1] TNA (Time & Action) Milestones**: Validate sample approvals, lab dips, strike-offs, and fabric gate dates.

### 3.2 Planning, FastReact & Production (`/planning`, `/garment`)
- [ ] **[P0] Line Loading & Capacity Planning**: Verify line allocation board, line efficiency targets, and bottleneck alerts.
- [ ] **[P1] Cutting Floor Execution**: Test marker efficiency, lay planning, bundle ticket generation, and fabric roll allocation.
- [ ] **[P1] Sewing & Finishing Floor Progress**: Hourly production tracking, line balancing, and DHU (Defects per Hundred Units) calculations.

### 3.3 Quality & Compliance (`/quality`)
- [ ] **[P0] 4-Point Fabric Inspection System**: Defect scoring calculation (points per 100 sq. yards) and Pass/Fail threshold flags.
- [ ] **[P1] In-line & End-line QC**: Traffic light system (Red/Yellow/Green), major vs minor defects tracking.
- [ ] **[P1] AQL 2.5 Final Audit**: Sampling plan generation according to ISO 2859-1 standards and digital sign-off.

### 3.4 Sourcing, Warehouse & Inventory (`/supply`)
- [ ] **[P0] Yarn & Fabric Inventory**: Roll tracking with QR/barcode, batch numbers, shade lot separation, and GSM verification.
- [ ] **[P1] Trims & Accessories Store**: Bin card allocation, minimum reorder level alerts, and issue-to-order reconciliation.
- [ ] **[P1] Gate Pass & Delivery Notes (Challan)**: Returnable vs non-returnable gate pass generation.

### 3.5 Finance & Commercial (`/finance`, `/trade`)
- [ ] **[P0] Export LC & BTMA Master LC Tracking**: Verify LC validity, maturity date alerts, amendment history, and utilization.
- [ ] **[P1] Commercial Invoices & Packing Lists**: HS Code auto-fill, gross vs net weight auto-sum, and customs declaration forms.

---

## 4. Settings & User Customization (`/settings`)

- [x] **[P0] 8 Settings Tabs**: Verify smooth switching across all tabs (`set-personal`, `set-context`, `set-language`, `set-appearance`, `set-notif`, `set-views`, `set-personalize`, `set-security`).
- [x] **[P0] Saved Views with Eye Icon**: Verify Eye icon (`D365Icon name="eye"`), "Add view" modal/action, and publish actions.
- [x] **[P1] Appearance & Density Toggle**: Verify Comfortable (40px) vs Compact (30px) row density changes table heights in real time.
- [ ] **[P1] Work Date Change & Context**: Test changing work date and company entity with toast notifications.
- [ ] **[P2] Notification Preferences**: Test toggles for sound, auto-dismiss, and category filters.

---

## 5. Technical Validation, Performance & Quality Assurance

- [x] **[P0] TypeScript Compilation**: Run `pnpm typecheck` (`tsc --noEmit`) — **Target: 0 errors**.
- [ ] **[P0] Next.js Build Check**: Run `pnpm build` to verify no static generation or SSR hydration mismatches.
- [ ] **[P1] Browser Console Audit**: Inspect DevTools console across all main routes for React key warnings, hydration errors, or missing asset 404s.
- [ ] **[P1] Visual & Design Token Check**: Validate contrast ratios, Dynamics 365 colors (`var(--navy-900)`, `var(--primary-600)`, `var(--gold-500)`), and font stacks (`Segoe UI`, `Inter`).
- [ ] **[P2] Test Coverage & Automation**: Execute Playwright / subagent browser verification on critical workflows.

---

## Daily Progress Log
| Time | Module / Area | Description of Test or Fix | Status |
| :--- | :--- | :--- | :--- |
| **10:15** | `/buyer` | Dynamic navbar active pill (`.portal-nav a.active`) with scroll-spy | **Fixed & Verified** |
| **10:30** | `/buyer` | Interactive `AK` profile flyout with persona & navigation links | **Fixed & Verified** |
| **10:45** | `/buyer` | Language toggle (`English` / `বাংলা`) with toast feedback | **Fixed & Verified** |
| **10:50** | Root | TypeScript typecheck (`tsc --noEmit`) | **Passed (0 errors)** |
| **11:05** | `/buyer` | Dedicated in-portal Buyer Profile & Settings modal (A. Khan) | **Fixed & Verified** |
| **11:45** | `/login` | Fixed nested `<body>` hydration error in `LoginPage` | **Fixed & Verified** |
| **11:48** | `/commercial` | Replaced Base UI `Button` render prop with `buttonVariants` on `<Link>` | **Fixed & Verified** |
| **11:59** | `/buyer` | Removed "Buyer 360 overview" option from `AK` profile dropdown | **Fixed & Verified** |
| **12:25** | `/worker` | Complete interactive upgrade of Worker Self-Service (M33): 4 core actions (Leave, Salary, Grievance, Welfare), Payslip PDF preview, PIN lock, Worker switcher, Bilingual toggle | **Fixed & Verified** |
| **12:50** | `/worker` | Added top navigation bar (`portal-top`) with dynamic section pill tabs and interactive `SB` profile avatar with flyout menu & settings modal | **Fixed & Verified** |
