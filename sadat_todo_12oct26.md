# Master Documentation Audit Todo List — 12 Oct 2026

**Project:** Garments & Textile Industry ERP (GTex ERP)  
**Author:** Sadat  
**Date:** 12 October 2026  
**File:** sadat_todo_12oct26.md  
**Scope:** Comprehensive Audit and Cross-Verification of All Project Documentation  

---

## 1. Audit Objectives & Review Methodology

Today's objective is to review, cross-examine, and catalog every document in the repository and external reference corpus. For each document, verify:
1. **Traceability:** Alignment with the Master Module Catalogue (M01..M43) and Architecture Decision Records (ADR-0001..ADR-0017).
2. **Codebase Reality:** Concordance with live implementations under `backend/apps/`, database schemas, and Frappe v16 Desk visibility.
3. **Discrepancy Identification:** Gaps between specifications and codebase (e.g. DocType naming, missing fixtures, obsolete references).
4. **Actionability:** Clear identification of work items needed to bring code into compliance with specifications.

---

## 2. Group-by-Group Documentation Checklist

### Group 1: Master Architecture & Planning Specifications (`Planning Docs/`)
- [ ] **1.1 `00_All_Projects_Overview.md`**
  - [ ] Verify 10-app dependency graph (ADR-0003, ADR-0013: no sibling imports, all route via `grp_core`).
  - [ ] Check mapping of M01..M43 to the 10 `grp_*` apps.
  - [ ] Validate phase allocations (P01 through P06) and build surface estimations.
- [ ] **1.2 `SYSTEM_ARCHITECTURE.md`**
  - [ ] Audit multi-tier system topology (Next.js frontend, Frappe backend, MariaDB, Redis, WebSocket).
  - [ ] Check discrepancy note: `custom_*` naming in Section 6 vs canonical `grp_*` naming.
  - [ ] Review security architecture (RBAC, ABAC, encryption, audit logging).
- [ ] **1.3 `PROJECT_STATUS_ANALYSIS_REPORT.md`**
  - [ ] Re-verify the 6 baseline audit findings (port 8090 vs 8080, corpus location, unmerged branches, scaffold classification).
  - [ ] Confirm progress made since initial report (custom modules moved to `backend/apps/`, Docker containerization).
- [ ] **1.4 `CUSTOM_MODULE_CONTENTS_CHECKLIST.md`**
  - [ ] Verify common 7-layer checklist (Skeleton, Initialiser, DocTypes, Services, Whitelisted API, Desk Visibility, Automated Tests).
  - [ ] Audit module-specific contents for each of the 10 apps.
  - [ ] Check critical traps documentation (missing `__init__.py`, JSON placement outside `doctype/`, `modified` timestamp bumps).
- [ ] **1.5 `FULL_FLOW_TEST_USER_MANUAL.md`**
  - [ ] Audit end-to-end journey steps from Buyer Enquiry -> Style BOM -> Sourcing -> Cutting -> Sewing -> Finishing -> Shipment.
  - [ ] Validate acceptance criteria for cross-module transactions.

---

### Group 2: The 10 Module Breakdown Documents (`Planning Docs/01..10`)
- [ ] **2.1 `01_Module_Breakdown_grp_core.md` (M40..M43 — 120 Capabilities)**
  - [ ] Cross-examine M40 (Platform Core, Effective Dating, Inventory Dimensions, Vocabularies).
  - [ ] Cross-examine M41 (Integration Idempotency, Buyer/Supplier Portals, Grievance Cases).
  - [ ] Cross-examine M42..M43 (Audit Logs, Legal Hold, DPIA, Change Management).
  - [ ] Cross-check against `sadat_core_match_report_12oct26.md` findings (CORE-01..CORE-18).
- [ ] **2.2 `02_Module_Breakdown_grp_commercial.md` (M01..M03, M08..M09)**
  - [ ] Check Buyer Master and Sales Contract specifications (M01).
  - [ ] Check Enquiry to Order and Ship Window schemas (M02).
  - [ ] Check Merchandiser Workbench, Demand Forecasting, and Line Capacity plans (M03, M08, M09).
- [ ] **2.3 `03_Module_Breakdown_grp_garment.md` (M04..M07, M18..M21)**
  - [ ] Check Style, Season, Style Variant, and Tech Pack definitions (M04..M06).
  - [ ] Check Cost Sheet, IE Standard Time, and Line Balancing specifications (M07, M18).
  - [ ] Check Cutting Room Marker/Lay/Bundle definitions (M19) with standard bundle ID format.
  - [ ] Check Sewing MES, Bundle Scan, Hourly Board, and Finishing specifications (M20, M21).
- [ ] **2.4 `04_Module_Breakdown_grp_textile.md` (M14..M17)**
  - [ ] Check Spinning and Weaving specifications (M14, M15: Fibre Lot, Yarn Lot, Fabric Roll).
  - [ ] Check Wet Processing and Printing specifications (M16, M17: Dye Recipe, Stenter Log, Screen).
- [ ] **2.5 `05_Module_Breakdown_grp_supplychain.md` (M10..M13)**
  - [ ] Check Sourcing and Supplier Scorecard specifications (M10).
  - [ ] Check Warehouse Bin, Put Away, Pick List, and Gate Pass schemas (M11, M12).
  - [ ] Check Subcontracting and Job Work Order schemas (M13).
  - [ ] Verify `uom_library.py` conversion standard.
- [ ] **2.6 `06_Module_Breakdown_grp_export.md` (M24..M25)**
  - [ ] Check Bond License, Utilization, Passbook, and Shipping Bill schemas (M24).
  - [ ] Check Commercial Invoice, Packing List, Bill of Lading, and DPP specifications (M25).
- [ ] **2.7 `07_Module_Breakdown_grp_finance.md` (M26..M28)**
  - [ ] Check Accounting Governance and Credit Limit rules (M26).
  - [ ] Check Treasury, LC, Back-to-Back LC, and Bank Reconciliation schemas (M27).
  - [ ] Check Fixed Assets and IFRS 16 Lease Contract schemas (M28).
- [ ] **2.8 `08_Module_Breakdown_grp_bdlocal.md` (M29A..F, M30..M33)**
  - [ ] Check Bangladesh VAT & Tax (16 Mushak DocTypes: 4.1..4.10, 6.1..6.10, 9.1).
  - [ ] Check Worker Lifecycle, Grade, and Transfer schemas (M30).
  - [ ] Check Attendance, Overtime (BLA 2006 rules), and Payroll schemas (M31, M32).
  - [ ] Check Worker Welfare, Daycare, Clinic, and Safety schemas (M33).
- [ ] **2.9 `09_Module_Breakdown_grp_compliance.md` (M22..M23, M34..M37)**
  - [ ] Review implementation against 15 service engines delivered on 08 Oct.
  - [ ] Verify AQL sampling (ANSI/ASQ Z1.4), CAPA, Calibration, Audit Findings, and ETP standards.
- [ ] **2.10 `10_Module_Breakdown_grp_mis.md` (M38..M39)**
  - [ ] Check KPI definitions, anti-gaming controls, and 12 persona dashboards (M38).
  - [ ] Check AI Governance, OCR, and Document Classification schemas (M39).

---

### Group 3: Setup Manuals & Technical Implementation Guides
- [ ] **3.1 `grp_garment_IMPLEMENTATION_GUIDE.md`**
  - [ ] Verify step-by-step garment app installation and configuration sequence.
  - [ ] Check field mapping between Tech Pack and Style BOM.
- [ ] **3.2 `CHART_OF_ACCOUNTS_ADD_CHILD_SETUP_MANUAL.md`**
  - [ ] Review standard accounting hierarchy and child account provisioning rules.
- [ ] **3.3 `COMPANY_BRANCH_BANK_ACCOUNTING_SETUP_MANUAL.md`**
  - [ ] Review multi-company, multi-branch, and bank account isolation setup.
- [ ] **3.4 App-Level Installation Guides (`SPRINT0_INSTALL.md`)**
  - [ ] Audit `backend/apps/grp_core/SPRINT0_INSTALL.md`.
  - [ ] Audit `backend/apps/grp_garment/SPRINT0_INSTALL.md`.

---

### Group 4: Frontend, Prototype & General Documentation
- [ ] **4.1 Root & Frontend READMEs**
  - [ ] Review root [README.md](file:///Users/laptopparadise/Downloads/Textile/README.md).
  - [ ] Review [frontend/README.md](file:///Users/laptopparadise/Downloads/Textile/frontend/README.md).
  - [ ] Review [frontend/docs/page-comparison.md](file:///Users/laptopparadise/Downloads/Textile/frontend/docs/page-comparison.md).
- [ ] **4.2 Prototype Documentation**
  - [ ] Review [prototype/README.md](file:///Users/laptopparadise/Downloads/Textile/prototype/README.md).
  - [ ] Review [prototype/COVERAGE.md](file:///Users/laptopparadise/Downloads/Textile/prototype/COVERAGE.md).
- [ ] **4.3 Demo Reference**
  - [ ] Review [gtex_demo/README.md](file:///Users/laptopparadise/Downloads/Textile/gtex_demo/README.md).

---

### Group 5: External Master Reference Corpus Alignment
- [ ] **5.1 Alignment with `/Users/laptopparadise/Downloads/garments & Texttile Industry 2/`**
  - [ ] `garments_ERP_ Module/` (M01..M43 functional specifications).
  - [ ] `primary_doc/` (`03_Master_Module_Catalogue.md`, `00_INDEX.md`).
  - [ ] `project_doc/` (02_Architecture_and_Design, 04_Build_and_Configuration).
  - [ ] `Implementation roadmap/` and `Project_URD/`.

---

### Group 6: Live Codebase Cross-Audit & Findings Tracking
- [ ] **6.1 Review `sadat_core_match_report_12oct26.md` Work Orders**
  - [ ] Verify P0 tickets (CORE-01 missing DocTypes, CORE-02 provider registry).
  - [ ] Verify P1 tickets (CORE-03..CORE-08 fixtures, test assertions, desk links).
  - [ ] Verify P2 tickets (CORE-09..CORE-18 capabilities).
- [ ] **6.2 Compile Comprehensive Daily Audit Report (`sadat_report_12oct26.md`)**
  - [ ] Document audit findings, discrepancies, coverage metrics, and actionable roadmap.
