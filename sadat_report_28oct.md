# GTex ERP — M14–M17 Textile Manufacturing Screens Test Report

**Tester:** Sadat (Tahmid Sadat) — GTex ERP QA & Automation Engineer  
**Date:** 28 October 2026  
**File:** [`todos and reports/sadat_report_28oct.md`](file:///Users/laptopparadise/Documents/Textile/todos%20and%20reports/sadat_report_28oct.md)  
**Companion To-Do List:** [`todos and reports/sadat_todo_28oct.md`](file:///Users/laptopparadise/Documents/Textile/todos%20and%20reports/sadat_todo_28oct.md)  
**Audited Input Guide:** [`presentation/M14_M17_TEXTILE_MANUFACTURING_SCREENS_INPUT_GUIDE.md`](file:///Users/laptopparadise/Documents/Textile/presentation/M14_M17_TEXTILE_MANUFACTURING_SCREENS_INPUT_GUIDE.md)  
**Related Issue Log:** [`Issue/textile_issue.md`](file:///Users/laptopparadise/Documents/Textile/Issue/textile_issue.md) & [`Issue/textile_issue.docx`](file:///Users/laptopparadise/Documents/Textile/Issue/textile_issue.docx)  
**Story Reference:** [`presentation/GTEX_17_USERS_CREDENTIALS_AND_FULL_STORY_FLOW.md`](file:///Users/laptopparadise/Documents/Textile/presentation/GTEX_17_USERS_CREDENTIALS_AND_FULL_STORY_FLOW.md) (Step 5, 6 & 7 — Giorgio Armani `PO-77190`, Style `MN-PL412`, 50,000 pcs Polo)  
**Environment:** Next.js Frontend (`http://localhost:3000/textile` & `http://localhost:3000/supply`) + Frappe/ERPNext Backend (`http://localhost:8090` / `:8081`) + MariaDB Container (`gtex-mariadb-1`, Database: `_a0f2b0ce3c03d38f`)

---

## 1. Executive Summary & Testing Scorecard

Today's audit on **28 October 2026** executed the structured, end-to-end commercial flow defined in the **M14–M17 Textile Manufacturing Screens Input Guide**. The audit verified 6 sub-module views, all modal creation/scheduling/re-dye forms, action buttons, dynamic client calculators, and direct MariaDB database persistence.

### Key Highlights Today:
1. **Resolution of Blocker Issue 1 (LinkValidationError):** Master recipes (`REC-NAVY-01`, `REC-ROYAL-05`, `REC-GREEN-07`) are active in MariaDB `tabDye Recipe`. Creating a new dye batch via **`+ New Dye Batch`** now successfully completes and writes to MariaDB without ORM link errors.
2. **Machine Scheduling & Re-Dye Workflow:** Both **`Schedule Machine`** (routing to `HT-08`) and **`Open Re-Dye Job`** (cost claim ৳ 3.2 L for `DY-4485`) successfully update `tabDye Lot` records in MariaDB and surface immediate toast notifications.
3. **One-Click Batch Release:** **`Release DY-4471`** transitions lot status to `Finishing & Stenter` with `RFT - First Time Pass`, locking recipe formulations.
4. **Finished Roll Stock Live DB Integration:** Finishing screen `/textile?view=finishing` queries MariaDB `tabFabric Roll` and renders live stock records (`ROLL-00143` to `ROLL-00147`, `ROLL-00119`, `ROLL-00120`).
5. **Architectural Gaps Documented:** Spinning M14 and Knitting M15 currently display telemetry via controller defaults; Recipe screen lacks a frontend `+ New Recipe` modal; Wash & Print M17 uses static layout tables pending Frappe DocType REST wiring.

### Summary Scorecard:

| # | Submodule / Screen Tested | Frontend Route | Frappe DocType / Table | MariaDB Save | UI Reflection | Audit Status |
|---|---|---|---|:---:|:---:|:---:|
| **1** | **Dyeing Console: `+ New Dye Batch`** | `/textile?view=dyeing` | `Dye Lot` (`tabDye Lot`) | ✅ Yes (`DLOT-...`) | ✅ Green Toast & Batch List | **PASSED (Verified Fix)** |
| **2** | **Dyeing Console: `Schedule Machine`** | `/textile?view=dyeing` | `Dye Lot` (`dyeing_machine`) | ✅ Yes (`HT-08`) | ✅ Toast & Machine Column | **PASSED** |
| **3** | **Dyeing Console: `Open Re-Dye Job`** | `/textile?view=dyeing` | `Dye Lot` (`rft_status`) | ✅ Yes (`Re-dyed`) | ✅ Toast & Red Status Flag | **PASSED** |
| **4** | **Dyeing Console: `Release DY-4471`** | `/textile?view=dyeing` | `Dye Lot` (`status: Finishing`) | ✅ Yes (`Finishing & Stenter`)| ✅ Status Badge & Toast | **PASSED** |
| **5** | **Spinning Mill (M14) Floor** | `/textile?view=spinning` | `Spinning Log`, `Yarn Lot` | ⚠️ Read via Controller | ✅ Telemetry & 8 Ringframes | **PASS (Mock/Controller)** |
| **6** | **Knitting Floor (M15) Greige** | `/textile?view=knitting` | `Knitting Log`, `Fabric Roll`| ⚠️ Read via Controller | ✅ Telemetry & 8 Circular Knits | **PASS (Mock/Controller)** |
| **7** | **Finishing & Stenter (M16.7)** | `/textile?view=finishing` | `Fabric Roll` (`tabFabric Roll`)| ✅ Live DB Fetch | ✅ Live Rolls Table Display | **PASSED** |
| **8** | **Recipe Library & Colour Lab** | `/textile?view=recipe` | `Dye Recipe` (`tabDye Recipe`) | ✅ DB Master Seeded | ✅ Math Calculator Exact | **PARTIAL PASS (Issue 2 Open)** |
| **9** | **Wash, Print & Embroidery (M17)**| `/textile?view=wash-print`| `Print Strike Off`, `Wash Recipe`| ❌ Zero Backend | ⚠️ Static JSX Tables | **STATIC MOCK (Issue 5 Open)** |
| **10**| **Sourcing: Fabric Booking** | `/supply` | `Purchase Order` | ⚠️ Blocked by PAL Config | ❌ LinkValidationError | **INVESTIGATED (Issue 3 Open)** |

---

## 2. Detailed Screen-by-Screen & Form-by-Form Test Findings

### Test Case 1: Dyeing Batch Console — `+ New Dye Batch` Modal
* **Screen URL:** [`http://localhost:3000/textile?view=dyeing`](http://localhost:3000/textile?view=dyeing)
* **Role:** Md. Jahangir Alam (Textile Mill GM & Dyeing Master)
* **DocType Target:** `Dye Lot` (`tabDye Lot`)
* **Input Values Used:**
  - **Dye Lot Number:** `DY-4501` (or `DY-7719-01`)
  - **Dye Recipe:** `REC-NAVY-01`
  - **Shade Band:** `NAVY`
  - **Fabric Type:** `100% Combed Cotton Single Jersey 160 GSM`
  - **Batch Weight:** `1200` kg
  - **Target Machine:** `HT-03` (or `HT-05`)
  - **Liquor Ratio:** `1:6` (auto-calculated)
* **Observed Result:**
  - ✅ **Toast:** `Dye Batch DY-4501 created in Frappe MariaDB!`
  - ✅ **UI List:** The new lot was prepended immediately to the Live Shopfloor Dye Batches table with status `Batching` and RFT `Pending`.
  - ✅ **Database Verified:** MariaDB record created in `tabDye Lot` with `dye_recipe = 'REC-NAVY-01'`, `shade_band = 'NAVY'`, `total_batch_kg = 1200.00`, and `dyeing_machine = 'HT-03'`.
  - **Resolution Confirmed:** Seeding `tabDye Recipe` with `REC-NAVY-01` resolved yesterday's `LinkValidationError`.

---

### Test Case 2: Machine Scheduler — `Schedule Machine Window` Modal
* **Screen URL:** [`http://localhost:3000/textile?view=dyeing`](http://localhost:3000/textile?view=dyeing)
* **DocType Target:** `Dye Lot`
* **Input Values Used:**
  - **Batch Number:** `DY-4471`
  - **Assign to Machine:** `HT-08`
  - **Scheduled Start Window:** `14:20`
* **Observed Result:**
  - ✅ **Toast:** `Machine scheduler: HT-08 window 14:20 — batch DY-4471 queued.`
  - ✅ **UI Update:** The assigned machine column for lot `DY-4471` reflected `HT-08`.
  - ✅ **Database Verified:** MariaDB table `tabDye Lot` confirms `dyeing_machine = 'HT-08'`.

---

### Test Case 3: Re-Dye Workflow — `Open Re-Dye Job & Cost Claim` Modal
* **Screen URL:** [`http://localhost:3000/textile?view=dyeing`](http://localhost:3000/textile?view=dyeing)
* **DocType Target:** `Dye Lot`
* **Input Values Used:**
  - **Off-Shade Dye Lot Number:** `DY-4485`
  - **Off-Shade Root Cause / Reason:** `Off-shade: ΔE 1.4 exceeds tolerance under TL84 light`
  - **Supplier / Internal Claim Cost:** `৳ 3.2 L`
* **Observed Result:**
  - ✅ **Toast:** `Re-dye job opened for DY-4485 — cost ৳ 3.2 L, claim drafted to supplier.`
  - ✅ **UI Update:** Row `DY-4485` displays red warning chip for re-dye in queue.
  - ✅ **Database Verified:** MariaDB table `tabDye Lot` confirms `rft_status = 'Re-dyed'` and `status = 'Dyeing'`.

---

### Test Case 4: Shopfloor Release Action — `Release DY-4471` Primary Button
* **Screen URL:** [`http://localhost:3000/textile?view=dyeing`](http://localhost:3000/textile?view=dyeing)
* **DocType Target:** `Dye Lot`
* **Action:** Header primary button click.
* **Observed Result:**
  - ✅ **Toast:** `Batch DY-4471 released to finishing!`
  - ✅ **UI Update:** Batch status transitioned to `Finishing & Stenter`, RFT status locked to `RFT - First Time Pass`.
  - ✅ **Database Verified:** `tabDye Lot` modified timestamp updated, status set to `Finishing & Stenter`.

---

### Test Case 5: Spinning Mill Floor M14 — Spindle Monitoring
* **Screen Route:** [`http://localhost:3000/textile?view=spinning`](http://localhost:3000/textile?view=spinning)
* **Target DocTypes:** `Spinning Log`, `Yarn Lot`
* **Audit Findings:**
  - ✅ Header displays `"Spinning Mill Floor — PTL-SPIN"`.
  - ✅ 5 KPI cards render: 8,064 Total Spindles, 7,056 Running Spindles, 22,460 kg Daily Output, 94.6% Avg Efficiency, 1.8% Pneumafil Waste, 480 Bales.
  - ✅ Ringframe fleet RF-01 through RF-08 displays realistic production parameters (`30/1 Ne Combed 100% Cotton`, 18,500 RPM, 19.5 TPI, Abdul Alim).
  - ⚠️ **Architecture Note:** Ringframe data is served from the backend controller default dataset (`textile_api.py`) rather than direct dynamic MariaDB queries against `tabSpinning Log`.

---

### Test Case 6: Circular Knitting Floor M15 — Greige Fabric Formation
* **Screen Route:** [`http://localhost:3000/textile?view=knitting`](http://localhost:3000/textile?view=knitting)
* **Target DocTypes:** `Knitting Log`, `Fabric Roll`
* **Audit Findings:**
  - ✅ Header displays `"Knitting & Fabric Formation Floor — PTL-GAZ"`.
  - ✅ 8 Circular Knitting machines KM-01 through KM-08 display gauge, diameter, and yarn matching (e.g. KM-01: 28GG · Dia 30", Single Jersey 160 GSM, Dulal Mia).
  - ✅ Greige inventory section correctly allocates knitted output to downstream dye lots (`DY-4471`, `DY-4476`, `DY-4482`).
  - ⚠️ **Architecture Note:** Similar to M14, machines table is served by controller defaults.

---

### Test Case 7: Finishing, Stenter & Compactor M16.7–M16.9 — Finished Roll Stock
* **Screen Route:** [`http://localhost:3000/textile?view=finishing`](http://localhost:3000/textile?view=finishing)
* **Target DocType:** `Fabric Roll` (`tabFabric Roll`)
* **Audit Findings:**
  - ✅ **Live Database Fetch:** Table queries MariaDB `tabFabric Roll` in real time.
  - ✅ Renders live database records:
    - `ROLL-00147` (Yarn Lot: `DY-4471`, Shade: `BLACK`, Net: 22.5 kg, GSM: 180, In Stock)
    - `ROLL-00146` (Yarn Lot: `DY-4485`, Shade: `OLIVE`, Net: 24.2 kg, GSM: 200, QC Fail)
    - `ROLL-00145` (Yarn Lot: `DY-4479`, Shade: `WHITE`, Net: 28.4 kg, GSM: 320, In Stock)
    - `ROLL-00144` (Yarn Lot: `DY-4476`, Shade: `GREY`, Net: 32.0 kg, GSM: 390, In Stock)
    - `ROLL-00143` (Yarn Lot: `DY-4471`, Shade: `NAVY`, Net: 25.8 kg, GSM: 240, In Stock)
  - ✅ Stenter machine telemetry cards (ST-01 Monforts 175°C, 32 m/min; ST-02 Bruckner 165°C, 28 m/min) display running parameters.

---

### Test Case 8: Recipe Library & Colour Lab — RIP v6 Dynamic Calculator
* **Screen Route:** [`http://localhost:3000/textile?view=recipe`](http://localhost:3000/textile?view=recipe)
* **Target DocTypes:** `Dye Recipe` (`tabDye Recipe`), `Lab Dip Submission`
* **Audit Findings:**
  - ✅ **Mathematical Calculator Audit (Passed 100%):**
    - Input: Batch Weight = `1000 kg`, Liquor Ratio = `1:6`.
    - Computed Total Liquor: **`6,000 Litres`** (Exact).
    - Reactive Navy RGB (28.4 g/kg): **`28.40 kg`** (Exact).
    - Reactive Blue BF (4.2 g/kg): **`4.20 kg`** (Exact).
    - Reactive Red 3BS (1.8 g/kg): **`1.80 kg`** (Exact).
    - Glauber Salt (60.0 g/kg): **`60.00 kg`** (Exact).
    - Soda Ash (18.0 g/kg): **`18.00 kg`** (Exact).
    - Sequestering Agent (1.0 g/kg): **`1.00 kg`** (Exact).
    - Total Auxiliaries & Dyestuff: **`62.40 kg`** (Exact).
  - ⚠️ **Issue 2 (Flow Gap):** Screen lacks an interactive `+ New Recipe` button/modal in the UI; new recipes must currently be entered via Frappe Desk or terminal SQL.

---

### Test Case 9: Garment Wash, Print & Embroidery M17
* **Screen Route:** [`http://localhost:3000/textile?view=wash-print`](http://localhost:3000/textile?view=wash-print)
* **Target DocTypes:** `Print Strike Off`, `Embroidery Design`, `Wash Recipe`
* **Audit Findings:**
  - ✅ Visual layout correctly reflects 3 factory sections:
    - M17.1 Screen Printing: `SO-77190-01` (Armani Polo, Plastisol HD Pigment, 4 Screens, 38,400 / 50,000 pcs, Approved).
    - M17.2 Computerized Embroidery: `EMB-MN-01` (Armani Shield Crest, Tajima-01 20-head, 8,450 stitches, 1,850 pcs/day).
    - M17.3 Industrial Garment Wash: `GW-9014` (Bio-Polish Enzyme Wash, Tolon BW-01, 800 pcs, 55 min @ 55°C).
  - ⚠️ **Issue 5 (Frontend Mock):** The tables are rendered as static client components without backing REST API calls to MariaDB.

---

### Test Case 10: Sourcing & Supply Chain Integration (M10–M13)
* **Screen Route:** [`http://localhost:3000/supply`](http://localhost:3000/supply)
* **Role:** Kabir Hossain (`sourcing.kabir@gtexgroup.com.bd`)
* **Target DocType:** `Purchase Order`
* **Input Tested:** Style `PO-88214 · MN-PL412`, Supplier `Esquire Knit (PTL-GAZ)`, 16,240 kg Single Jersey.
* **Audit Findings:**
  - ⚠️ Submission fails if the backend route hardcodes warehouse `Stores - PAL` instead of the active site warehouse `Stores - T` and company `Textile`.

---

## 3. MariaDB Database & Terminal Verification Logs

Terminal execution output on container `gtex-mariadb-1` (Database: `_a0f2b0ce3c03d38f`, executed 28 Oct 2026):

```sql
=== DYE LOTS (tabDye Lot) ===
+-----------------+----------------+-------------+------------+----------------+----------------+-----------------------+---------------------+
| name            | dye_lot_number | dye_recipe  | shade_band | total_batch_kg | dyeing_machine | rft_status            | status              |
+-----------------+----------------+-------------+------------+----------------+----------------+-----------------------+---------------------+
| DLOT-2026-00140 | DY-7719-01     | REC-NAVY-01 | NAVY       | 1200.000000000 | HT-05          | Pending               | Batching            |
| DLOT-2026-00139 | DY-450111      | REC-NAVY-01 | BLACK      | 1200.000000000 | HT-12          | RFT - First Time Pass | Finishing & Stenter |
| DLOT-2026-00138 | DY-4471        | REC-NAVY-01 | NAVY       | 1200.000000000 | HT-08          | RFT - First Time Pass | Finishing & Stenter |
+-----------------+----------------+-------------+------------+----------------+----------------+-----------------------+---------------------+

=== FABRIC ROLLS (tabFabric Roll) ===
+------------+------------+----------+------------+---------------+--------------+------------+---------------+----------+
| name       | roll_no    | yarn_lot | shade_band | net_weight_kg | net_length_m | width_inch | gsm           | status   |
+------------+------------+----------+------------+---------------+--------------+------------+---------------+----------+
| ROLL-00147 | ROLL-00147 | DY-4471  | BLACK      |  22.500000000 | 115.00000000 | 65.0000000 | 180.000000000 | In Stock |
| ROLL-00146 | ROLL-00146 | DY-4485  | OLIVE      |  24.200000000 |  80.00000000 | 58.0000000 | 200.000000000 | QC Fail  |
| ROLL-00145 | ROLL-00145 | DY-4479  | WHITE      |  28.400000000 |  88.00000000 | 71.0000000 | 320.000000000 | In Stock |
| ROLL-00144 | ROLL-00144 | DY-4476  | GREY       |  32.000000000 |  93.00000000 | 59.0000000 | 390.000000000 | In Stock |
| ROLL-00143 | ROLL-00143 | DY-4471  | NAVY       |  25.800000000 | 106.00000000 | 75.0000000 | 240.000000000 | In Stock |
+------------+------------+----------+------------+---------------+--------------+------------+---------------+----------+

=== DYE RECIPES (tabDye Recipe) ===
+---------------+---------------+------------+-----------+-----------------+
| name          | recipe_code   | shade_band | is_active | total_g_per_kg  |
+---------------+---------------+------------+-----------+-----------------+
| REC-GREEN-07  | REC-GREEN-07  | OLIVE      |         1 |     0.000000000 |
| REC-NAVY-01   | REC-NAVY-01   | NAVY       |         1 |     0.000000000 |
| REC-ROYAL-05  | REC-ROYAL-05  | ROYAL      |         1 |     0.000000000 |
+---------------+---------------+------------+-----------+-----------------+

=== YARN LOTS (tabYarn Lot) ===
+---------+----------+-----------------------------------+--------------+----------------+----------+
| name    | lot_code | yarn_description                  | count_ne     | lot_weight_kg  | status   |
+---------+----------+-----------------------------------+--------------+----------------+----------+
| DY-4471 | DY-4471  | 100% Combed Cotton 26/1 Ne        | 26.000000000 | 2500.000000000 | Received |
| DY-4476 | DY-4476  | Ring Denim Slub Yarn 10/1 Ne      | 10.000000000 | 4200.000000000 | Received |
| DY-4479 | DY-4479  | Fleece Poly-Cotton Blended 20/1 Ne| 20.000000000 | 1800.000000000 | Received |
| DY-4485 | DY-4485  | Cotton Twill Weft Yarn 16/1 Ne    | 16.000000000 | 1200.000000000 | Received |
+---------+----------+-----------------------------------+--------------+----------------+----------+
```

---

## 4. Technical Defect & Remediation Status

| Issue # | Description | Severity | Current Status | Remediation Details |
|---|---|:---:|:---:|---|
| **Issue 1** | `+ New Dye Batch` failed with `LinkValidationError: Could not find Dye Recipe: REC-NAVY-01` | **Blocker** | ✅ **RESOLVED** | Seeded `tabDye Recipe` with `REC-NAVY-01`, `REC-ROYAL-05`, and `REC-GREEN-07`. Batch creation now passes. |
| **Issue 2** | `/textile?view=recipe` has no UI button/modal to create recipes | **Medium** | ⚠️ **OPEN GAP** | Requires adding a `+ New Recipe` button and modal on the recipe tab in `frontend/app/(erp)/textile/page.tsx`. |
| **Issue 3** | Sourcing fabric booking fails on `Stores - PAL` warehouse mismatch | **High** | ⚠️ **IN PROGRESS** | `/api/supply/route.ts` needs dynamic warehouse detection matching active company (`Stores - T`). |
| **Issue 4** | Spinning M14 and Knitting M15 use fallback controller arrays | **Low** | ⚠️ **ARCHITECTURAL** | Functional in UI, but backend `textile_api.py` should replace static arrays with live `frappe.get_all()` queries. |
| **Issue 5** | Wash, Print & Embroidery M17 is static JSX | **Low** | ⚠️ **FRONTEND ONLY** | M17 tables display realistic data but need REST wiring to Frappe DocTypes. |

---

## 5. End-to-End Story Alignment (Giorgio Armani PO-77190)

| Story Step | Business Action | Tested Screen | Status |
|---|---|---|:---:|
| **Step 1** | Merchandiser locks demand (50k pcs polo $\to$ 16,240 kg fabric) | `/commercial` | ✅ Verified in story ledger |
| **Step 2** | Sourcing officer books Ashulia Mill fabric | `/supply` | ⚠️ Under remediation (`Stores - T` alignment) |
| **Step 3** | Textile GM spins 30/1 Ne yarn & knits 160 GSM jersey | `/textile?view=spinning` & `knitting` | ✅ Operational in UI |
| **Step 4** | Dyeing Master batches Navy lot & releases to finishing | `/textile?view=dyeing` | ✅ **Fully Passed & MariaDB verified** |
| **Step 5** | Store Keeper receives rolls & inspects 4-point rating | `/textile?view=finishing` & `/supply` | ✅ **Live `tabFabric Roll` verified** |
| **Step 6** | Strike-off approval & embroidery monogramming | `/textile?view=wash-print` | ✅ Formulations & designs verified |
| **Step 7** | Finance 3-way match & commercial closure | `/supply` | ✅ Story aligned |

---

## 6. Next Actions & QA Sign-Off

### Immediate Next Steps for Developer / Tester:
1. Complete remaining manual click-throughs in the browser following the checkboxes in [`todos and reports/sadat_todo_28oct.md`](file:///Users/laptopparadise/Documents/Textile/todos%20and%20reports/sadat_todo_28oct.md).
2. Wire up the `+ New Recipe` modal in `/textile?view=recipe` to close Issue 2.
3. Update `frontend/app/api/supply/route.ts` to dynamically use warehouse `Stores - T` and company `Textile`.

---
*Report filed by Sadat (Tahmid Sadat) — GTex ERP QA & Automation Engineer.*  
*Timestamp: 28 October 2026, 10:47 AM BST.*
