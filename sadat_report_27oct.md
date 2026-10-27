# GTex ERP — M14–M17 Textile Manufacturing Screens Test Report

**Tester:** Sadat  
**Date:** 27 October 2026  
**Audited Guide:** [`presentation/M14_M17_TEXTILE_MANUFACTURING_SCREENS_INPUT_GUIDE.md`](file:///Users/laptopparadise/Documents/Textile/presentation/M14_M17_TEXTILE_MANUFACTURING_SCREENS_INPUT_GUIDE.md)  
**Related Issue Log:** [`Issue/textile_issue.md`](file:///Users/laptopparadise/Documents/Textile/Issue/textile_issue.md) & [`Issue/textile_issue.docx`](file:///Users/laptopparadise/Documents/Textile/Issue/textile_issue.docx)  
**Story Reference:** [`presentation/GTEX_17_USERS_CREDENTIALS_AND_FULL_STORY_FLOW.md`](file:///Users/laptopparadise/Documents/Textile/presentation/GTEX_17_USERS_CREDENTIALS_AND_FULL_STORY_FLOW.md) (Step 5, 6 & 7)  
**Environment:** Next.js Frontend (`http://localhost:3000/textile` & `http://localhost:3000/supply`) + Frappe/ERPNext Backend (`http://localhost:8090` / `:8081`)

---

## 1. Executive Summary & Testing Scorecard

Today's testing session conducted a comprehensive, field-by-field audit of **Domain 05: Textile Manufacturing & Wet Processing (M14–M17)** and its cross-module integration with **Sourcing & Inventory (M10–M13)** based on the input guide.

The testing evaluated 6 submodule views, 4 interactive modals/actions, live database persistence in MariaDB, and end-to-end commercial story consistency (Giorgio Armani `PO-77190`, Style `MN-PL412`, 50,000 pcs Polo).

### Summary Scorecard:

| # | Submodule / Screen Tested | Frontend Route | Frappe DocType / Table | MariaDB Save | UI Reflection | Overall Status |
|---|---|---|---|:---:|:---:|:---:|
| **1** | **Dyeing Console: `+ New Dye Batch`** | `/textile?view=dyeing` | `Dye Lot` (`tabDye Lot`) | ❌ Failed (Link Error) | ❌ Error Toast | **BLOCKER (Issue 1)** |
| **2** | **Dyeing Console: `Schedule Machine`** | `/textile?view=dyeing` | `Dye Lot` (`dyeing_machine`) | ✅ Yes | ✅ Yes (Queued Toast) | **PASSED** |
| **3** | **Dyeing Console: `Open Re-Dye Job`** | `/textile?view=dyeing` | `Dye Lot` (`rft_status`) | ✅ Yes | ✅ Yes (Toast & DB) | **PASSED** |
| **4** | **Dyeing Console: `Release DY-4471`** | `/textile?view=dyeing` | `Dye Lot` (`status: Finishing`) | ✅ Yes | ✅ Yes (Recipe locked) | **PASSED** |
| **5** | **Spinning Mill (M14) Floor** | `/textile?view=spinning` | `Spinning Log`, `Yarn Lot` | ❌ No DB Query | ⚠️ Static Mock Display | **MOCK ONLY (Issue 4)** |
| **6** | **Knitting Floor (M15) Greige** | `/textile?view=knitting` | `Knitting Log`, `Fabric Roll`| ❌ No DB Query | ⚠️ Static Mock Display | **MOCK ONLY** |
| **7** | **Finishing & Stenter (M16.7)** | `/textile?view=finishing` | `Fabric Roll` (`tabFabric Roll`)| ✅ Live DB Fetch | ⚠️ Read-only Roll Grid | **PARTIAL PASS** |
| **8** | **Recipe Library & Colour Lab** | `/textile?view=recipe` | `Dye Recipe` | ❌ No Input UI | ✅ Client Calculator OK | **GAP (Issue 2)** |
| **9** | **Wash, Print & Embroidery (M17)**| `/textile?view=wash-print`| `Print Strike Off`, `Wash Recipe`| ❌ Zero Backend | ⚠️ Static JSX Tables | **STATIC MOCK** |
| **10**| **Sourcing: `New fabric booking`** | `/supply` | `Purchase Order` | ❌ Failed (Link Error) | ❌ Error Toast | **BUG (Issue 3)** |

---

## 2. Detailed Screen-by-Screen & Form-by-Form Test Findings

### Test Case 1: Dyeing Batch Console — `+ New Dye Batch` Modal
* **Screen URL:** `http://localhost:3000/textile?view=dyeing`
* **Role:** Jahangir Alam (Textile Mill GM & Dyeing Master)
* **DocType Target:** `Dye Lot` (`tabDye Lot`)
* **Input Data Tested:**
  - Dye Lot Number: `DY-4501` / `DY-450111`
  - Dye Recipe: `REC-NAVY-01`
  - Shade Band: `NAVY`
  - Fabric Type: `100% Combed Cotton Single Jersey 160 GSM`
  - Batch Weight: `1200` kg
  - Target Machine: `HT-03` / `HT-01`
* **Observed Result:**
  - ❌ **Submission Failure:** Clicking **`Create in Frappe DB`** threw a red error toast:
    `LinkValidationError: Could not find Dye Recipe: REC-NAVY-01`.
  - **Root Cause:** In Frappe ORM, `dye_recipe` on `Dye Lot` is a mandatory Link field. `tabDye Recipe` was completely empty in the MariaDB database, causing referential integrity validation (`validate_link()`) to fail.

---

### Test Case 2: Machine Scheduler — `Schedule Machine Window` Modal
* **Screen URL:** `http://localhost:3000/textile?view=dyeing`
* **DocType Target:** `Dye Lot`
* **Input Data Tested:**
  - Batch Number: `DY-4471`
  - Assign to Machine: `HT-08`
  - Scheduled Start Window: `14:20`
* **Observed Result:**
  - ✅ **Success:** Toast appeared: `"Machine scheduler: HT-08 window 14:20 — batch DY-4471 queued."`
  - ✅ **Database Verified:** MariaDB record updated `dyeing_machine = 'HT-08'`.

---

### Test Case 3: Re-Dye Workflow — `Open Re-Dye Job & Cost Claim` Modal
* **Screen URL:** `http://localhost:3000/textile?view=dyeing`
* **DocType Target:** `Dye Lot`
* **Input Data Tested:**
  - Off-Shade Lot: `DY-4485`
  - Root Cause: `Off-shade: ΔE 1.4 exceeds tolerance under TL84 light`
  - Supplier Claim Cost: `৳ 3.2 L`
* **Observed Result:**
  - ✅ **Success:** Re-dye case opened; toast confirmed claim drafted to supplier.
  - ✅ **Database Verified:** `rft_status` set to `Re-dyed` and status returned to `Dyeing`.

---

### Test Case 4: Shopfloor Release Action — `Release DY-4471` Button
* **Screen URL:** `http://localhost:3000/textile?view=dyeing`
* **DocType Target:** `Dye Lot`
* **Action:** Direct primary header button click.
* **Observed Result:**
  - ✅ **Success:** Status transitioned to `Finishing`, recipe locked, RFT marked as `RFT Pass`. Finished fabric rolls made available for Central Store dispatch.

---

### Test Case 5: Spinning Mill M14 — Ringframe Machine Status & Spindle Monitoring
* **Screen URL:** `http://localhost:3000/textile?view=spinning`
* **DocType Target:** `Spinning Log` (`tabSpinning Log`), `Yarn Lot` (`tabYarn Lot`)
* **Audit Findings:**
  1. **Read-Only Screen:** There is **no interactive button or modal** (e.g., no `+ Log Spindle Doff` or `+ Intake Cotton Bales`). Users cannot record production from the UI.
  2. **Zero MariaDB Querying:** `backend/apps/grp_textile/grp_textile/textile_api.py` (`get_spinning_console_data()`) returns a static hardcoded Python array of 8 ringframes (`RF-01` to `RF-08`).
  3. **Direct DB Injection Test:** Manually inserted row `RF-99` (`60/1 Ne Super Combed`, Output: `5,000 kg`) directly into `tabSpinning Log` in MariaDB. Re-calling `/api/textile?action=spinning` completely ignored MariaDB and returned the same hardcoded 8 rows.
  4. **KPI Cards:** All 5 KPI cards (8,064 Spindles, 23,350 kg Yarn, 94.2% Efficiency, 1.8% Pneumafil Waste, 412 Bales) are derived from the static Python array.

---

### Test Case 6: Knitting Floor M15 — Circular Knitting Greige Output
* **Screen URL:** `http://localhost:3000/textile?view=knitting`
* **DocType Target:** `Knitting Log` (`tabKnitting Log`), `Fabric Roll` (`tabFabric Roll`)
* **Audit Findings:**
  - Similar to Spinning M14, the 8 circular knitting machines (`KM-01` to `KM-08`) are hardcoded in the backend Python controller.
  - There is no interactive `+ Doff Greige Roll` or inspection entry form on the screen.
  - No live roll generation occurs from this screen.

---

### Test Case 7: Finishing & Stenter M16.7 — Finished Fabric Roll Stock
* **Screen URL:** `http://localhost:3000/textile?view=finishing`
* **DocType Target:** `Fabric Roll` (`tabFabric Roll`)
* **Audit Findings:**
  - ✅ **Live DB Integration:** The roll inventory table queries MariaDB `tabFabric Roll` and correctly displays real records (e.g., `ROLL-00119`, `ROLL-00120`, `ROLL-00013`, `ROLL-00014`, `ROLL-00015` with width 64", 160 GSM, and `QC Pass` status).
  - ⚠️ **Limitation:** The screen is strictly a viewing inventory table; there is no UI action to create or release rolls. Stenter machines `ST-01` and `ST-02` display static mock telemetry.

---

### Test Case 8: Recipe Library & Colour Lab — RIP v6
* **Screen URL:** `http://localhost:3000/textile?view=recipe`
* **DocType Target:** `Dye Recipe` (`tabDye Recipe`), `Lab Dip Submission` (`tabLab Dip Submission`)
* **Audit Findings:**
  - ✅ **Client-side Calculator Works:** Changing Batch Weight (`1000 kg`) and Liquor Ratio (`1:6`) dynamically calculates Total Liquor (`6,000 L`) and scales chemical dosages (Reactive Navy RGB 28.4 kg, Glauber Salt 60.0 kg, Soda Ash 18.0 kg).
  - 🚨 **Critical Flow Gap:** The screen **lacks a `+ New Recipe` button or modal**. A colorist cannot add or approve new recipes from the frontend UI.
  - Furthermore, the recipe list is returned from a static Python array rather than querying `tabDye Recipe`.

---

### Test Case 9: Garment Wash, Print & Embroidery M17
* **Screen URL:** `http://localhost:3000/textile?view=wash-print`
* **DocType Target:** `Print Strike Off`, `Embroidery Design`, `Wash Recipe`
* **Audit Findings:**
  - Completely static JSX mock-up.
  - The 3 tables (M17.1 Screen Printing Strike-off, M17.2 Tajima Embroidery, M17.3 Tolon Belly Washers) are hardcoded directly into the frontend React component.
  - No REST API is called and no Frappe database tables are queried or populated.

---

### Test Case 10: Sourcing & Fabric Booking (M10–M13 Cross-Module Flow)
* **Screen URL:** `http://localhost:3000/supply`
* **Role:** Kabir Hossain (Sourcing & Purchase Officer)
* **DocType Target:** `Purchase Order` (`tabPurchase Order`)
* **Input Data Tested:**
  - Style / Order: `PO-88214 · MN-PL412`
  - Mill / Supplier: `Esquire Knit (PTL-GAZ)`
  - Article: `Single Jersey 160 GSM`
  - Quantity: `16,240 kg`
  - Need-by: `2026-09-30`
* **Observed Result:**
  - ❌ **Submission Failure:** Form threw `frappe.exceptions.LinkValidationError: Could not find Stores - PAL`.
  - **Root Cause:**
    1. **Warehouse Mismatch:** API code (`frontend/app/api/supply/route.ts`) hardcoded target warehouse to `Stores - PAL`, whereas the registered Frappe warehouse is `Stores - T`.
    2. **Company Mismatch:** Hardcoded company `Padma Apparels Ltd.`, while the registered company is `Textile`.
    3. **Missing Supplier & Item:** `Esquire Knit (PTL-GAZ)` was missing from `tabSupplier`, and the fabric item code lacked UOM conversion in Frappe master tables.

---

## 3. Summary of Documented Technical Issues

Detailed logs and screenshots are preserved in [`Issue/textile_issue.md`](file:///Users/laptopparadise/Documents/Textile/Issue/textile_issue.md):

1. **Issue 1 (Blocker):** `+ New Dye Batch` throws `LinkValidationError: Could not find Dye Recipe: REC-NAVY-01` due to an unseeded `tabDye Recipe` table.
2. **Issue 2 (Flow Gap):** `/textile?view=recipe` has no UI button/modal to create recipes, forcing reliance on manual DB inserts.
3. **Issue 3 (Bug):** `/supply` fabric booking fails on `Stores - PAL` and `Padma Apparels Ltd.` company mismatch against MariaDB master data.
4. **Issue 4 (Mock Architecture):** Spinning Mill M14 and Knitting Floor M15 use 100% hardcoded Python arrays, ignoring `tabSpinning Log` and `tabKnitting Log` in MariaDB.
5. **Issue 5 (Frontend Mock):** Wash, Print & Embroidery M17 is hardcoded static JSX without backend API routes or DocType connections.

---

## 4. End-to-End Story Alignment (Giorgio Armani PO-77190)

| Story Step | Business Action | Tested Screen | Status |
|---|---|---|:---:|
| **Step 1** | Merchandiser locks demand (50k pcs polo $\to$ 16,240 kg fabric) | `/commercial` | ✅ Verified |
| **Step 2** | Sourcing officer runs MRP & books Ashulia Mill fabric | `/supply` | ⚠️ Blocked by Issue 3 (Warehouse mismatch) |
| **Step 3** | Textile GM spins 30/1 Ne yarn & knits 160 GSM jersey | `/textile?view=spinning` & `knitting` | ⚠️ Display only (Mock backend) |
| **Step 4** | Dyeing Master batches Navy lot & releases to finishing | `/textile?view=dyeing` | ⚠️ Blocked by Issue 1 (`REC-NAVY-01` seed missing) |
| **Step 5** | Store Keeper receives rolls & runs 4-point inspection | `/supply` | ✅ Verified (`tabFabric Roll` traceable) |
| **Step 6** | Subcontract vendor uploads enzyme wash delivery note | `/vendor` | ✅ Verified |
| **Step 7** | Finance conducts 3-way match & clears payment | `/supply` | ✅ Verified |

---

## 5. Action Items & Developer Remediation Plan

### Backend Fixes (`backend/apps/grp_textile/`):
1. **Seed Master Dye Recipes:** Seed `REC-NAVY-01`, `REC-BLACK-04`, `REC-ROYAL-05`, `REC-GREY-02`, and `REC-OLIVE-03` into MariaDB `tabDye Recipe`.
2. **Connect Spinning & Knitting APIs:**
   - In `textile_api.py`, replace static arrays with live queries:
     ```python
     db_spindles = frappe.get_all("Spinning Log", fields=["*"], order_by="creation desc", limit=20)
     ```
3. **Fix Sourcing Warehouse & Company:**
   - In `frontend/app/api/supply/route.ts`, query active warehouse (`Stores - T`) and company (`Textile`) dynamically instead of hardcoding `Stores - PAL` and `Padma Apparels Ltd.`.
4. **Seed Supplier & Item Masters:**
   - Ensure `Esquire Knit (PTL-GAZ)` exists in `tabSupplier` and standard fabrics exist in `tabItem` with `Kg` UOM.

### Frontend UI Fixes (`frontend/app/(erp)/textile/`):
1. **Add `+ New Recipe` Button & Modal:**
   - Add a primary button on `/textile?view=recipe` allowing lab colorists to create and save new recipes directly to MariaDB.
2. **Add Production Logging Forms:**
   - Add `+ Log Spindle Doff` modal on `/textile?view=spinning`.
   - Add `+ Doff Greige Roll` modal on `/textile?view=knitting`.
3. **Wire M17 to Backend APIs:**
   - Connect Wash, Print & Embroidery tables to `tabPrint Strike Off`, `tabEmbroidery Design`, and `tabWash Recipe`.

---
*Report filed by Sadat — GTex ERP QA & Automation Engineer.*

