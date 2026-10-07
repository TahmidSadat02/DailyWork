# Sadat's QA & Testing Todo — 6 October 2026

**Project:** Garments & Textile Industry ERP (Enterprise Platform)  
**Target Branch:** `sadat` (Sync from `origin/sharna`)  
**Scope:** Frontend (Next.js), Backend (Frappe v16 + ERPNext custom `grp_*` apps), Portals & Navigation  

---

## 0. Initial Setup & Branch Sync

> **Note:** The `sadat` branch was originally at the initial commit. To test the latest code, sync with the `sharna` branch:
> ```bash
> git pull origin sharna
> ```

- [ ] **Sync Branch:** Pull latest code from `origin/sharna` into `sadat`
- [ ] **Docker / Backend Services:**
  - [ ] Check `docker-compose.yml` (`docker compose up -d`)
  - [ ] Verify MariaDB & Redis services running
  - [ ] Seed demo user if running bench: `bench execute grp_core.demo_user.make`
- [ ] **Frontend Dev Server:**
  - [ ] `cd frontend && pnpm install` (or `npm install`)
  - [ ] Verify `.env.local` configured with backend URL
  - [ ] Run `pnpm dev` and ensure server starts on `http://localhost:3000`

---

## 1. Authentication & Shell / Navigation Test

- [ ] **Login Screen (`/login`):**
  - [ ] UI layout, logo, responsive rendering
  - [ ] Form validation (empty inputs, invalid credentials)
  - [ ] Successful login redirect to Role Center (`/`)
- [ ] **Top Bar & Navigation Shell (`/`):**
  - [ ] User menu dropdown & Profile avatar
  - [ ] **Language Toggle:** Test switching between **English** and **বাংলা (bn-BD)**
  - [ ] **Density Toggle:** Comfortable vs. Compact layout density
  - [ ] **Tell-Me Global Search:** Verify search input opens, searches module catalogue
  - [ ] **Sidebar Navigation:** Collapsible state, icons, active route highlighting

---

## 2. Core ERP Module Screen Checks

Test page loading, layout, KPI tiles, data tables, and action buttons across each domain:

### 2.1 Commercial, Orders & Product
- [ ] **Commercial Hub (`/commercial`):** KPI cards, inquiry and order status
- [ ] **Orders Grid (`/commercial/orders`):** PO list, Colour x Size grid display
- [ ] **TNA Calendar (`/commercial/tna`):** Time & Action milestone status
- [ ] **Product Dev & Costing (`/product`):** Tech Pack, BOM, marker consumption

### 2.2 Planning, Manufacturing & Quality
- [ ] **Planning (`/planning`):** Master production schedule, line capacity
- [ ] **Supply Chain (`/supply`):** Raw material inventory, yarn/fabric roll tracking
- [ ] **Textile Manufacturing (`/textile`):** Spinning, knitting, weaving, dyeing status
- [ ] **Garment Manufacturing MES (`/garment`):** Cutting, sewing bundle tracking, finishing
- [ ] **Quality & Lab (`/quality`):** 4-point fabric inspection, inline QC, final AQL pass/fail gate

### 2.3 Trade, Finance & Governance
- [ ] **Trade & Export (`/trade`):** Export/Import LCs, Bond Passbook, shipping docs
- [ ] **Finance & Accounts (`/finance`):** General ledger, receivables, payables
- [ ] **People & HR (`/people`):** Employee directory, shift attendance, payroll
- [ ] **Governance & Compliance (`/governance`):** RSC/BSCI audit logs, safety compliance
- [ ] **Plants & Assets (`/plants`):** Machine maintenance, boiler/ETP logs
- [ ] **Insights & BI (`/insights`):** Executive dashboards, daily KPI summaries
- [ ] **Settings (`/settings`):** System parameters and role configurations

---

## 3. Dedicated Portals & Worker ESS

- [ ] **Worker ESS Portal (`/worker`):**
  - [ ] Mobile-first / touch layout
  - [ ] Attendance history, OT hours, payslip view
- [ ] **Buyer Portal (`/buyer`):**
  - [ ] External buyer dashboard
  - [ ] PO status tracking, inspection report access
- [ ] **Vendor / Supplier Portal (`/vendor`):**
  - [ ] Purchase order acknowledgment
  - [ ] Material delivery and challan tracking

---

## 4. UI/UX & Responsive Testing

- [ ] **Desktop Resolution (1920x1080 / 1440x900):** Layout symmetry, table horizontal scrolling
- [ ] **Tablet / Mobile View (Chrome DevTools):** Hamburger menu, collapsible sidebar, touch targets
- [ ] **Error States & Notifications:** Toast popups (Sonner) on error/success actions
- [ ] **Number & Currency Formatting:** BDT (৳) and USD ($) numeral formatting in both English and Bangla

---

## 5. Bug & Issue Log

| # | Module / Route | Issue Description | Severity (High / Med / Low) | Status |
|---|---|---|---|---|
| 1 | | | | Open |
| 2 | | | | Open |
| 3 | | | | Open |
| 4 | | | | Open |
| 5 | | | | Open |

---

## 6. Daily Wrap-Up
- [ ] Record test observations and attach screenshots
- [ ] Commit any test notes/fixtures to branch `sadat`
- [ ] Push updates: `git push origin sadat`
