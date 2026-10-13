# GTex ERP Export Trade (`grp_export`) Testing Todo List — 13 Oct 2026

**Project:** Garments & Textile Industry ERP (GTex ERP)  
**Author / Tester:** Sadat  
**Date:** 13 October 2026  
**File:** `todos and reports/sadat_todo_13oct26.md`  
**Target Module:** `grp_export` (Bonded Warehouse, Customs, Export Shipment, Logistics, EDI & Digital Product Passport)  
**Reference Document:** `todos and reports/sadat_report_13oct26.md` & `Planning Docs/06_Module_Breakdown_grp_export.md`  
**Live Desk Environment:** `http://localhost:8090/desk` (Site: `dev.localhost`)  
**Workspace Route:** `http://localhost:8090/desk#workspace/GRP%20Export%20Trade`  

---

## 1. Testing Objectives & Execution Protocol

Today's objective is to execute functional, visual, and statutory verification of the `grp_export` module across its 2 core domains (**M24** Customs Bond & Declarations and **M25** Export Logistics & Traceability):
1. **Navigation & Desk Integration:** Confirm the workspace layout, shortcuts, metric tiles, and DocType lists render properly on Frappe Desk.
2. **Customs Bonded Warehouse (M24):** Verify bond licences, quota ceiling calculation, passbook inward/outward ledger entries, and the NBR bond reconciliation report.
3. **Statutory Bangladesh Customs Clearances (M24):** Audit ASYCUDA CUSDEC Shipping Bills, Bangladesh Bank EXP Forms, BGMEA/BKMEA UD/UP permits, and GSP REX certificates.
4. **Export Logistics & Shipping (M25):** Audit export shipment consolidation, commercial invoicing with Incoterms, packing list gross/net/CBM math, and SOLAS VGM container stuffing.
5. **EDI & Digital Product Passport (M25):** Validate EDIFACT DESADV and ANSI X12 856 ASN payloads with idempotency constraints, and inspect EU ESPR Art. 13 CIRPASS JSON DPP records.
6. **Zero-Regression Test Runner:** Confirm all 50 automated backend unit and integration tests pass cleanly.

---

## 2. Interactive Step-by-Step Testing Checklist

### Phase 1: Desk Workspace, Navigation & Visibility
- [ ] **1.1 Desk App Launcher Tile**
  - [ ] Open `http://localhost:8090/desk`.
  - [ ] Confirm **GRP Export Trade** tile is visible with the export trade icon.
  - [ ] Click tile and verify it routes directly to `/desk#workspace/GRP%20Export%20Trade`.
- [ ] **1.2 Workspace Dashboard Elements**
  - [ ] Open `http://localhost:8090/desk#workspace/GRP%20Export%20Trade`.
  - [ ] Verify **KPI Metric Shortcuts**:
    - [ ] *Active Bond Licences* shortcut count
    - [ ] *Pending Shipping Bills* shortcut count
    - [ ] *Live Export Shipments* shortcut count
    - [ ] *DPP Issued* shortcut count
  - [ ] Verify **Functional Cards**:
    - [ ] Card 1: *Bonded Warehouse & Customs (M24)*
    - [ ] Card 2: *Export Shipment & Invoicing (M25)*
    - [ ] Card 3: *Logistics, EDI & Traceability (M25)*

---

### Phase 2: Customs Bonded Warehouse & Passbook Audit (M24)
- [ ] **2.1 Bond Licence Register**
  - [ ] Open `http://localhost:8090/desk#Form/Bond%20Licence/BL-00068` (Dhaka Customs Bond Commissionerate).
  - [ ] Check primary fields: Licence No (`4471-BC`), Company, Effective Date, Expiry Date.
  - [ ] Check financial limits: Total Import Ceiling (USD), Total Utilized (USD), Bank Guarantee Amount (BDT).
  - [ ] Inspect child table *Bond Licence Item*: verify raw yarn and fabric sanctioned quotas.
  - [ ] Open secondary licence: `http://localhost:8090/desk#Form/Bond%20Licence/BL-00069` (Chittagong Bond).
- [ ] **2.2 Bond Passbook Entries & Material Accounting**
  - [ ] Open `http://localhost:8090/desk#List/Bond%20Passbook%20Entry/List`.
  - [ ] Filter by Entry Type: *Inward (Import Receipt)*. Verify raw material quantity credit.
  - [ ] Filter by Entry Type: *Outward (Export Deduction)*. Verify Input-Output Coefficient (IOC) quota reduction and Duty Saved calculation (BDT).
  - [ ] Confirm link to parent *Bond Licence* and underlying *Bill of Entry* or *Shipping Bill*.
- [ ] **2.3 NBR Bond Reconciliation Report**
  - [ ] Navigate to `http://localhost:8090/desk#query-report/Bond%20Reconciliation%20Report`.
  - [ ] Select Company: `GTex Apparels Ltd.`.
  - [ ] Select Bond Licence: `BL-00068`.
  - [ ] Set As of Date: Today.
  - [ ] Click **Generate / Refresh**.
  - [ ] Verify report table columns: *Item Code*, *Imported Qty*, *Export Deducted Qty*, *Allowable Wastage %*, *Book Balance*, *Physical Balance*, *Variance*.
  - [ ] Confirm variance is within statutory NBR 0.01 tolerance.

---

### Phase 3: Statutory Bangladesh Customs Clearances (M24)
- [ ] **3.1 Shipping Bill (ASYCUDA World CUSDEC)**
  - [ ] Open `http://localhost:8090/desk#Form/Shipping%20Bill/SB-2026-00075`.
  - [ ] Check customs station assignment: `Chittagong Custom House (Customs Station: BDCGP)`.
  - [ ] Verify core export attributes: *Declared Gross Weight*, *Declared Net Weight*, *Total FOB Amount (USD)*.
  - [ ] Inspect child table *Shipping Bill Item*: check HS Code, unit rate, and country of destination.
  - [ ] Review ASYCUDA CUSDEC XML transmission status.
- [ ] **3.2 Bangladesh Bank EXP Form**
  - [ ] Open `http://localhost:8090/desk#Form/EXP%20Form/EXP-2026-00072`.
  - [ ] Verify mandatory statutory fields:
    - [ ] Bangladesh Bank EXP Number (`EXP-2026-00072`)
    - [ ] Authorised Dealer (AD) Bank & Branch Code
    - [ ] Port of Shipment (`Chittagong Sea Port`)
    - [ ] Invoice Value & Currency
    - [ ] Repatriation Deadline (120-day compliance rule)
- [ ] **3.3 BGMEA / BKMEA Utilization Declaration (UD) & Permission (UP)**
  - [ ] Open `http://localhost:8090/desk#Form/UD%20Document/UD-2026-00073`.
  - [ ] Verify issuing association: `BGMEA`.
  - [ ] Check linked Master Export LC / Sales Contract number.
  - [ ] Inspect child table *UD Item*: check raw fabric and yarn consumption quotas allocated against the order.
  - [ ] Open UP Document List: `http://localhost:8090/desk#List/UP%20Document/List`.
- [ ] **3.4 Certificate of Origin & GSP REX Scheme**
  - [ ] Open `http://localhost:8090/desk#List/Certificate%20of%20Origin/List`.
  - [ ] Open `http://localhost:8090/desk#List/GSP%20Claim/List`.
  - [ ] Verify Export Preference Scheme (EU GSP / UK DCTS / Canada GPT).
  - [ ] Confirm Registered Exporter (REX) number and Origin Statement text.

---

### Phase 4: Export Shipment, Logistics & Commercial Invoicing (M25)
- [ ] **4.1 Export Shipment Record**
  - [ ] Open `http://localhost:8090/desk#Form/Export%20Shipment/EXP-SHP-2026-00086`.
  - [ ] Check logistics headers:
    - [ ] Port of Loading (POL): `Chittagong (BDCGP)`
    - [ ] Port of Discharge (POD): `Rotterdam (NLRTM)`
    - [ ] Carrier / Shipping Line name
    - [ ] Vessel Name & Voyage No.
    - [ ] Estimated Time of Departure (ETD) & Arrival (ETA)
  - [ ] Inspect child table *Export Shipment Item*: verify garment style, order quantity, and CTN count.
  - [ ] Spot-check additional shipments: `EXP-SHP-2026-00087` through `EXP-SHP-2026-00090`.
- [ ] **4.2 Commercial Invoice**
  - [ ] Open `http://localhost:8090/desk#Form/Commercial%20Invoice/EXP-INV-2026-00082`.
  - [ ] Verify Incoterms 2020 token: `FOB`, `CIF`, or `CFR`.
  - [ ] Check financial breakdown: Total FOB Value, Freight, Insurance, and Net Invoice Total.
  - [ ] Verify banking instructions: Beneficiary Bank, SWIFT, and Master LC number.
- [ ] **4.3 Container Stuffing & SOLAS VGM Verification**
  - [ ] Open `http://localhost:8090/desk#List/Container%20Stuffing/List`.
  - [ ] Inspect Container No. and Customs Seal No.
  - [ ] Verify IMO SOLAS Verified Gross Mass (VGM):
    - [ ] Tare weight of container
    - [ ] Cargo gross weight
    - [ ] Total VGM verified weight and method (Method 1: weigh filled box / Method 2: tally contents).
- [ ] **4.4 Packing List & Carton Summary**
  - [ ] Open `http://localhost:8090/desk#List/Packing%20List/List`.
  - [ ] Verify total cartons, total gross weight (kg), net weight (kg), and total CBM.
  - [ ] Check *Carton Packing List* child table / sub-breakdown: carton range, pieces per carton.
- [ ] **4.5 Transport Bill Documents**
  - [ ] Open `http://localhost:8090/desk#List/Bill%20of%20Lading/List` (Ocean Freight).
  - [ ] Open `http://localhost:8090/desk#List/Airway%20Bill/List` (Air Freight).
  - [ ] Confirm Freight Forwarder, Shipper, and Consignee links.

---

### Phase 5: Buyer EDI & Digital Product Passport (DPP) Traceability (M25)
- [ ] **5.1 Digital Product Passport (EU ESPR Art. 13)**
  - [ ] Open `http://localhost:8090/desk#Form/DPP%20Record/DPP-2026-00093`.
  - [ ] Verify garment metadata: Style Reference, Garment Category, Brand/Buyer.
  - [ ] Inspect sustainability metrics:
    - [ ] Fibre Composition (e.g. 100% Organic Cotton)
    - [ ] Carbon Footprint per garment (kg CO2e)
    - [ ] Water Footprint per garment (Liters)
    - [ ] Recyclability & Circularity rating
  - [ ] Verify CIRPASS JSON payload schema view.
  - [ ] Check QR code / public passport URL reference.
- [ ] **5.2 Chain of Custody & Traceability Ledger**
  - [ ] Open `http://localhost:8090/desk#List/Chain%20of%20Custody/List`.
  - [ ] Open `http://localhost:8090/desk#List/Origin%20Chain%20Entry/List`.
  - [ ] Verify cryptographic traceability: SHA-256 hash linkage connecting:
    - Spinning Mill (Fiber batch) → Knitting/Weaving (Fabric roll) → Dyeing (Color lot) → Sewing (Cut bundle) → Export Shipment.
- [ ] **5.3 Buyer EDI Transmissions & Idempotency**
  - [ ] Open `http://localhost:8090/desk#List/EDI%20Transaction%20Log/List`.
  - [ ] Verify logged outgoing messages:
    - [ ] EDIFACT D96A DESADV (for H&M, Inditex)
    - [ ] ANSI X12 856 ASN (for VF, US buyers)
  - [ ] Confirm each record has an `idempotency_key` preventing duplicate transmission.

---

## 3. Backend Automated Test Runner Verification

Verify that all Python test suites pass cleanly inside the backend container:

```bash
docker exec gtex-backend-1 bench --site dev.localhost run-tests --module grp_export
```

Or execute the standalone Python test runner:
```bash
python3 backend/apps/grp_export/tests/run_tests.py
```

- [ ] `test_m24_features.py` (Bond licence, passbook logic)
- [ ] `test_m24_statutory_trade.py` (Shipping bill, EXP, UD/UP, COO, GSP)
- [ ] `test_m25_export_shipment.py` (Shipment, container stuffing, VGM, invoices)
- [ ] `test_buyer_edi.py` (DESADV, ASN 856, idempotency)
- [ ] `test_dpp_and_traceability.py` (EU CIRPASS JSON, SHA-256 custody chain)
- [ ] **All 50/50 test cases: PASS**

---

## 4. Test Execution Bug & Observations Tracker

Record any issues, UI glitches, or discrepancy items found during today's session:

| # | Domain / DocType | Test Action Performed | Expected Outcome | Actual Result | Status (PASS / FAIL / BLOCKED) | Bug / Note |
|---|---|---|---|---|---|---|
| 1 | Desk Workspace | Loaded `/desk#workspace/GRP%20Export%20Trade` | All cards & shortcuts visible | Rendered cleanly with 4 KPI shortcuts | PASS | — |
| 2 | Bond Licence | Opened `BL-00068` | Sanctioned quotas display | | | |
| 3 | Bond Passbook | Inspected inward/outward ledger | Correct IOC deductions | | | |
| 4 | Bond Reconciliation | Ran report for `BL-00068` | Variance <= 0.01 | | | |
| 5 | Shipping Bill | Opened `SB-2026-00075` | ASYCUDA XML ready | | | |
| 6 | EXP Form | Opened `EXP-2026-00072` | 120-day repatriation date | | | |
| 7 | UD Document | Opened `UD-2026-00073` | BGMEA item allocation | | | |
| 8 | Export Shipment | Opened `EXP-SHP-2026-00086` | POL/POD vessel routing | | | |
| 9 | Commercial Invoice | Opened `EXP-INV-2026-00082` | FOB/CIF math calculated | | | |
| 10 | Container Stuffing | Checked container stuffing list | SOLAS VGM weight logged | | | |
| 11 | DPP Record | Opened `DPP-2026-00093` | CIRPASS JSON & QR code | | | |
| 12 | Chain of Custody | Checked custody trail list | SHA-256 hash valid | | | |
| 13 | Buyer EDI Log | Checked EDI transaction list | Idempotency keys enforced | | | |

---

## 5. Daily Testing Sign-Off

- **Tester:** Sadat  
- **Test Date:** 13 October 2026  
- **Total Test Cases:** 25 Manual Desk Checks + 50 Automated Unit Tests  
- **Passed:** ______ / 75  
- **Failed:** ______ / 75  
- **Sign-off Decision:** `[ ] APPROVED FOR MERGE` / `[ ] REVISION REQUIRED` / `[ ] BLOCKED`  
- **Tester Signature:** __________________________________
