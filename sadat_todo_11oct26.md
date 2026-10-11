# Backend Implementation Master Plan (Serial Build Order) — 11 Oct 2026

**Project:** Garments & Textile Industry ERP (GTex ERP)  
**Author:** Sadat  
**Date:** 11 October 2026  
**File:** sadat_todo_11oct26.md  

---

## 1. Master Build Sequence and Dependencies

The 10 custom Frappe apps must be built strictly in dependency order. No sibling app may directly import another; all cross-app interactions resolve via grp_core provider registry (ADR-0003, ADR-0013).

```
Build Serial:
1. grp_core         (Foundation - No dependency; all apps depend on grp_core)
2. grp_commercial   (Depends on: grp_core)
3. grp_garment      (Depends on: grp_core, grp_commercial)
4. grp_supplychain  (Depends on: grp_core, grp_commercial)
5. grp_compliance   (COMPLETED on 08 Oct 2026 - M22, M23, M34, M35, M36, M37)
6. grp_finance      (Depends on: grp_core, grp_commercial)
7. grp_bdlocal      (Depends on: grp_core, grp_finance; extends Frappe HR)
8. grp_textile      (Depends on: grp_core, grp_garment)
9. grp_export       (Depends on: grp_core, grp_commercial, grp_garment, grp_supplychain)
10. grp_mis         (Depends on: all apps; read-only consumer, built last)
```

---

## 2. Standard Quality Checklist for Each Module

For every custom module in the sequence, the following standards must be delivered:
- Skeleton: pyproject.toml, hooks.py, modules.txt, patches.txt, README.md
- Module initialiser: __init__.py inside scrubbed module directory
- DocType placement: Under <module>/doctype/<name>/ with .json, .py, .js, test_*.py
- Services: Pure Python business logic inside services/ (controllers only orchestrate)
- Whitelisted API: REST search, filter, and action methods in api.py for Next.js frontend
- Desk visibility: 4 layers (DocType, Workspace, Workspace Sidebar, Desktop Icon + add_to_apps_screen)
- Testing: Automated unit, negative gate, and syntax verification tests

---

## 3. Detailed Step-by-Step Module Todo List

### Step 1: grp_core — Platform Foundation (M40..M43)
Status: PENDING
- [ ] 1.1 Package Setup: Configure pyproject.toml, hooks.py, modules.txt, and public assets
- [ ] 1.2 Core DocTypes (M40):
  - [ ] Legal Entity, Operating Unit, Item Code Rule, UOM Conversion Rule
  - [ ] Warehouse Location, Inventory Dimension fixtures (Shade, Roll, Dye Lot, Cut Lot, Bundle)
  - [ ] Approval Matrix, Approval Request, Access Scope, ABAC Policy
  - [ ] Inter Company Rule, Effective Dated Rule, Document Repository, Vocabulary Master
- [ ] 1.3 Integration & Portal DocTypes (M41):
  - [ ] Integration Idempotency Record, Integration Webhook Event
  - [ ] Portal Access Token, Buyer Portal Scope, Supplier Portal Token, Supplier Portal Submission
  - [ ] ESS Identity, Grievance Case (schema-level non-deletable per STD-22)
- [ ] 1.4 Audit, Security & Ops DocTypes (M42..M43):
  - [ ] Audit Log, Legal Hold, DPIA, Incident Review, Change Request, Defect
- [ ] 1.5 Core Services & Architecture:
  - [ ] Provider Registry (register_interface, get_interface per ADR-0013)
  - [ ] Effective dating engine (resolve_version per SP-02)
  - [ ] Gate evaluation engine (check_gate per SP-01)
  - [ ] Idempotency validator (idempotency.py)
  - [ ] PartnerAdapter abstract base class (integrations/partner.py)
- [ ] 1.6 Whitelisted API: api.py exposing core registry, portal authentication, and vocabulary lookup
- [ ] 1.7 Desk Visibility: Workspace, Sidebar, Desktop Icon, and Apps Screen hook
- [ ] 1.8 Automated Tests: Unit and gate verification tests in tests/

---

### Step 2: grp_commercial — Buyer, Order Book, Demand & Capacity (M01..M03, M08..M09)
Status: PENDING
- [ ] 2.1 Package Setup: Configure hooks.py with required_apps = ["grp_core"]
- [ ] 2.2 Buyer Master DocTypes (M01):
  - [ ] Buyer, Buyer Brand, Buying Department, Destination Market, Buying Agent
  - [ ] Nominated Supplier, Disclosed Subcontractor, Buyer Price List, Sales Contract
- [ ] 2.3 Enquiry to Order DocTypes (M02):
  - [ ] Buyer Enquiry, Feasibility Check, Sales Order Item Grid, Order Ship Window
  - [ ] Order Amendment (immutable), Order Split, Sub Order, Order Hold
- [ ] 2.4 Merchandising & Planning DocTypes (M03, M08, M09):
  - [ ] Merchandiser Workbench, Buyer Concession, Demand Forecast, Master Production Schedule
  - [ ] Production Line, Line Capacity, Worker Line Assignment, Production Calendar
- [ ] 2.5 Services & Validation:
  - [ ] Gate validation on Sales Order (validate_gate_chain)
  - [ ] Original ship date lock
  - [ ] Order book status roll-up logic
- [ ] 2.6 Whitelisted API: Endpoints for Next.js Commercial & TNA consoles (/commercial/tna)
- [ ] 2.7 Desk Visibility: Commercial Workspace, Sidebar, and Desktop Icon
- [ ] 2.8 Automated Tests: Order gate and capacity calculation tests

---

### Step 3: grp_garment — PLM, Costing, Cutting, Sewing MES, Finishing (M04..M07, M18..M21)
Status: PENDING
- [ ] 3.1 Package Setup: Configure hooks.py with required_apps = ["grp_core", "grp_commercial"]
- [ ] 3.2 PLM & Sampling DocTypes (M04..M06):
  - [ ] Style, Style Variant, Style Attribute Definition, Season, Product Type
  - [ ] Tech Pack, Style BOM, Style BOM Line, Style BOM Operation
  - [ ] Sample Request, Fit Approval, Pre Production Sample, TNA Activity
- [ ] 3.3 Costing & IE DocTypes (M07, M18):
  - [ ] Cost Sheet, Cost Sheet Material/Operation/Overhead, Standard Time, Line Balancing Plan
- [ ] 3.4 Cutting Room DocTypes (M19):
  - [ ] Cut Order Plan, Marker Plan, Lay Plan, Spreading Log, Cutting Log
  - [ ] Bundle Definition with PRD-F068 standard identifier: {order}-{style}-{colour}-{size}-{cut_lot:04d}-{bundle:04d}
- [ ] 3.5 Sewing Floor & MES DocTypes (M20):
  - [ ] Bundle Scan, Production Output, Rework Log, Hourly Production Board, Downtime Log
- [ ] 3.6 Finishing & Packing DocTypes (M21):
  - [ ] Ironing Log, Packing Scan, Carton, Shipping Mark, Ex-Factory Checklist
- [ ] 3.7 Calculation Engines:
  - [ ] costing_engine.py, cutting_optimiser.py, cartonisation.py
- [ ] 3.8 Whitelisted API: MES scanning and floor data capture endpoints
- [ ] 3.9 Desk Visibility & Tests: Garment Workspace, Sidebar, and automated tests

---

### Step 4: grp_supplychain — Sourcing, Inventory, Warehouse, Subcontract (M10..M13)
Status: PENDING
- [ ] 4.1 Package Setup: Configure hooks.py with required_apps = ["grp_core", "grp_commercial"]
- [ ] 4.2 Sourcing & Procurement DocTypes (M10):
  - [ ] Supplier Category, Supplier Audit, Supplier Scorecard, Material Requirement Plan
- [ ] 4.3 Inventory & Warehouse DocTypes (M11, M12):
  - [ ] Inventory Dimension fixtures, Lot Genealogy Edge (append-only)
  - [ ] Warehouse Bin, Put Away Rule, Pick List, Store Indent, Gate Pass
- [ ] 4.4 Subcontracting DocTypes (M13):
  - [ ] Subcontractor, Job Work Order, Subcontract Material Issue, Process Loss Log
- [ ] 4.5 Core Services:
  - [ ] uom_library.py (System single source of truth for kg <-> metre conversion)
  - [ ] 3-way match validation hook
  - [ ] Negative-stock prevention guard
- [ ] 4.6 Whitelisted API: Supplier portal endpoints (/vendor) for PO, ASN, and invoice upload
- [ ] 4.7 Desk Visibility & Tests: Supply Chain Workspace, Sidebar, and tests

---

### Step 5: grp_compliance — Quality, Lab, Social Audit, EHS, Maintenance, ETP (M22..M23, M34..M37)
Status: COMPLETED (Verified on 08 Oct 2026)
- [x] 5.1 Package Setup: Configured hooks.py, pyproject.toml, modules.txt
- [x] 5.2 DocTypes Delivered: Defect Type, AQL Plan, Inspection Plan, Final Inspection, CAPA, Calibration Record, Audit Finding, EHS Incident, PM Schedule, Meter, Meter Reading, ETP Log
- [x] 5.3 Services Delivered: 15 engines including aql.py, defect_taxonomy.py, release_waiver.py, calibration.py, capa_engine.py, etp.py, carbon.py, maintenance.py, meter_capture.py
- [x] 5.4 Whitelisted API: api.py with AQL calculator, CAPA search, ETP check, Carbon accounting
- [x] 5.5 Desk Visibility: Workspace, Sidebar, Desktop Icon, and logo assets
- [x] 5.6 Automated Tests: 15/15 tests passing

---

### Step 6: grp_finance — Accounting, Treasury, Trade Finance, Assets (M26..M28)
Status: PENDING
- [ ] 6.1 Package Setup: Configure hooks.py with required_apps = ["grp_core", "grp_commercial"]
- [ ] 6.2 Accounting Governance DocTypes (M26):
  - [ ] Accounting Dimension, Credit Limit Rule, Inter Company Transaction, Consolidation Run
- [ ] 6.3 Treasury & Trade Finance DocTypes (M27):
  - [ ] Bank Statement Import, Bank Reconciliation Rule, Payment Run
  - [ ] Letter of Credit, Back To Back LC, TT Instruction, Trade Finance Limit
- [ ] 6.4 Fixed Assets DocTypes (M28):
  - [ ] Asset Category, Depreciation Method, Capital WIP, Lease Contract (IFRS 16)
- [ ] 6.5 Services & Security:
  - [ ] Daily freeze orchestrator (daily_freeze.py)
  - [ ] Real-time credit exposure calculation
  - [ ] Bank account number hash security (SEC-08)
- [ ] 6.6 Whitelisted API & Desk Visibility: Finance Workspace and reports
- [ ] 6.7 Automated Tests: Financial reconciliation and credit limit tests

---

### Step 7: grp_bdlocal — Bangladesh Statutory + HR / Payroll / Welfare (M29A..F, M30..M33)
Status: PENDING
- [ ] 7.1 Package Setup: Configure hooks.py; integrate with Frappe HR v16 base
- [ ] 7.2 Bangladesh VAT & Tax DocTypes (M29A..F):
  - [ ] Statutory Rate, Tax Instrument, Filing Calendar
  - [ ] 16 Mushak DocTypes: 4.1 through 4.10, 6.1, 6.2, 6.3, 6.7, 6.8, 6.10, and 9.1
  - [ ] VDS Rule, VDS Certificate, TDS Return, Salary Tax Slab
- [ ] 7.3 Worker Lifecycle DocTypes (M30):
  - [ ] Worker Badge, Worker Grade, Worker Transfer/Promotion/Separation, Disciplinary Action
- [ ] 7.4 Attendance & Payroll DocTypes (M31, M32):
  - [ ] Attendance Punch (gapless serial), OT Calculation, Shift Roster
  - [ ] Payroll Period, Payroll Run, Earning/Deduction Type, Festival Bonus, WPPF Provision
- [ ] 7.5 Worker Welfare DocTypes (M33):
  - [ ] Daycare Registration, Clinic Visit, Safety Incident, Grievance Record
- [ ] 7.6 Core Services:
  - [ ] High-volume payroll calculation engine (20,000 workers)
  - [ ] BLA 2006 labour law overtime and leave rule calculator
  - [ ] NID and PII data masking (STD-15)
- [ ] 7.7 Whitelisted API: Worker Self-Service endpoints (/worker)
- [ ] 7.8 Desk Visibility & Tests: BD Local Workspace and statutory tests

---

### Step 8: grp_textile — Spinning, Knitting, Weaving, Wet Processing, Printing (M14..M17)
Status: PENDING
- [ ] 8.1 Package Setup: Configure hooks.py with required_apps = ["grp_core", "grp_garment"]
- [ ] 8.2 Spinning & Weaving DocTypes (M14, M15):
  - [ ] Fibre Lot, Fibre Bale Receipt, Yarn Lot, Warp Beam, Weaving Log, Knitting Log, Fabric Roll
- [ ] 8.3 Wet Processing & Value-Added DocTypes (M16, M17):
  - [ ] Dye Recipe, Dyeing Log, Stenter Log, Chemical Inventory, Screen, Print Run Log, Wash Recipe
- [ ] 8.4 Services: Lot genealogy append-only edge writer and shade band verification
- [ ] 8.5 Whitelisted API & Desk Visibility: Textile Workspace and tests

---

### Step 9: grp_export — Bond, Customs, Shipping, EDI, Traceability (M24..M25)
Status: PENDING
- [ ] 9.1 Package Setup: Configure hooks.py with required_apps = ["grp_core", "grp_commercial", "grp_garment", "grp_supplychain"]
- [ ] 9.2 Bond & Customs DocTypes (M24):
  - [ ] Bond Licence, Bond Utilisation Entry, Bond Passbook Entry, Shipping Bill, EXP Form, UD/UP Document
- [ ] 9.3 Shipment & Traceability DocTypes (M25):
  - [ ] Shipment, Commercial Invoice, Packing List, Bill of Lading, Container Stuffing, DPP Record
- [ ] 9.4 Adapters & Services:
  - [ ] Buyer EDI adapters (DESADV, INVOIC)
  - [ ] ASYCUDA customs adapter
  - [ ] Digital Product Passport (DPP) payload generator (EU ESPR standard)
- [ ] 9.5 Whitelisted API & Desk Visibility: Export Workspace and shipment tracking tests

---

### Step 10: grp_mis — MIS, BI, KPI Engine, AI Layer (M38..M39)
Status: PENDING (Executed last)
- [ ] 10.1 Package Setup: Read-only consumer dependent on all preceding modules
- [ ] 10.2 KPI & Dashboard DocTypes (M38):
  - [ ] KPI Definition (anti-gaming control), KPI Value
  - [ ] 12 Persona Dashboards: Executive, Factory, Merchandiser, Finance, HR, Compliance, Buyer, Production, Quality, Sales, Procurement
- [ ] 10.3 AI Layer DocTypes (M39):
  - [ ] OCR Result, Document Classification, ML Model, Vector Store, AI Governance Record
- [ ] 10.4 Core Services:
  - [ ] Daily KPI refresh batch job
  - [ ] Hourly floor efficiency aggregator
  - [ ] 18 frozen Wave-1 executive reports
- [ ] 10.5 Whitelisted API & Desk Visibility: MIS Workspace and analytics tests
