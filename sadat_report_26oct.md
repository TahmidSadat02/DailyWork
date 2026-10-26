# GTex ERP — M08/M09 Planning & Demand Modules Test Report

**Tester:** Sadat  
**Date:** 26 October 2026  
**Audited Guide:** [`presentation/M08_DEMAND_LEDGER_AND_PLANNING_SCREENS_INPUT_GUIDE.md`](file:///Users/laptopparadise/Documents/Textile/presentation/M08_DEMAND_LEDGER_AND_PLANNING_SCREENS_INPUT_GUIDE.md)  
**Related Issue Log:** [`Issue/demand_issue.md`](file:///Users/laptopparadise/Documents/Textile/Issue/demand_issue.md)  
**Environment:** Next.js Frontend (`http://localhost:3000/planning`) + Frappe/ERPNext Backend (`http://localhost:8090` / `:8081`)

---

## 1. Executive Summary & Testing Scorecard

Today's end-to-end audit tested all 6 interactive planning forms and the Demand Ledger ingestion screen across **Domain 03: Demand Forecasting, S&OP & Master Production Schedule (M08)** and **Line Loading & Factory Capacity (M09)**.

### Summary Scorecard:

| # | Screen / Form Tested | Frontend Route | Frappe DocType | MariaDB Save | Frontend UI Reflection | Overall Status |
|---|---|---|---|:---:|:---:|:---:|
| **0** | **Demand Ledger (EDI 850 & Orders)** | `/planning?view=demand&subview=demand-ledger` | `Sales Order` | ❌ (Client State) | ⚠️ Partial (Counter only) | **Issue Found** |
| **1** | **+ New Forecast (Demand Forecast)** | `/planning?view=demand` | `Demand Forecast` | ✅ Yes | ✅ Yes (Dynamic Chart) | **PASSED** |
| **2** | **+ Schedule Plan (Line Loading Plan)** | `/planning?view=board` | `Line Loading Plan` | ✅ Yes | ⚠️ Partial (Status overlay) | **PASSED (with note)** |
| **3** | **+ New Line (Production Line)** | `/planning?view=board` | `Production Line` | ✅ Yes (`L-29` saved) | ❌ No (Stuck at 28 lines) | **BUG (Backend Filter)** |
| **4** | **+ New MPS Schedule (MPS)** | `/planning?view=mps` | `Master Production Schedule` | ✅ Yes (`MPS-2026-00130`) | ❌ No (Table has 5 dummy rows) | **BUG (API Array Merge)** |
| **5** | **+ Add Line Capacity (Line Capacity)** | `/planning?view=capacity` | `Line Capacity` | ✅ Yes (Record saved) | ❌ No (Table & KPI unchanged) | **BUG (Missing API Query)** |
| **6** | **+ New S&OP Cycle (S&OP Reconciliation)** | `/planning?view=sop` | `SOP Cycle` | ✅ Yes (`SOP-2026-TEST`) | ✅ Yes (Live Header & 4 KPI Cards) | **PASSED (Dynamic)** |

---

## 2. Detailed Form-by-Form Test Findings

### Test Case 0: Demand Ledger Screen (`?view=demand&subview=demand-ledger`)
* **Actions Tested:**
  1. Clicked **"Poll Buyer EDI (850)"** button.
  2. Created Sales Order `PO-44922` for buyer M&S in the Commercial Orders module.
* **Findings:**
  - **EDI Poll Behavior:** The counter increments in temporary React state (`React.useState(14)` $\to$ `17`), but resets back to 14 on browser reload (F5). No HTTP request is fired to backend.
  - **KPI Cards:** "Total Contract Value", "Confirmed vs Speculative", and "Ingestion Protocol" are static text strings.
  - **Buyer Order Book Table:** Hardcoded to 6 static sample rows (`PO-91007` to `PO-88231`). Newly created Sales Orders (`PO-44922`) do not populate here.

---

### Test Case 1: Form 1 — Demand Forecast (`+ New Forecast` Modal)
* **Screen:** `/planning?view=demand`
* **DocType:** `Demand Forecast` (`tabDemand Forecast`)
* **Input Data:**
  - Forecast Title: `AW 2026 Polo & Activewear Demand`
  - Model: `Buyer Committed Booking`
  - Forecast Qty: `500,000 pcs`
  - Forecast Value: `$2,500,000`
* **Findings:**
  - ✅ **Result:** Form submits via `save_demand_forecast` API and successfully saves to MariaDB.
  - ✅ **UI Reflection:** Consolidated forecasting chart dynamically reflects the new demand curve.

---

### Test Case 2: Form 2 — Line Loading Plan (`+ Schedule Plan` Modal)
* **Screen:** `/planning?view=board`
* **DocType:** `Line Loading Plan` (`tabLine Loading Plan`)
* **Input Data:**
  - Plan Name: `LLP-2026-03-PL412`, Line: `L-03`, Style: `MN-PL412`, PO: `PO-77190`
  - Daily Target: `2,100`, Target SMV: `14.5`, Planned Eff: `72%`, Status: `Scheduled`
* **Findings:**
  - ✅ **Result:** Saved successfully in MariaDB. Line `L-03` reflects the Armani order `PO-77190` and locks the schedule slot in the Gantt board.

---

### Test Case 3: Form 3 — Production Line (`+ New Line` Modal)
* **Screen:** `/planning?view=board`
* **DocType:** `Production Line` (`tabProduction Line`)
* **Input Data:**
  - Line Code: `L-29`
  - Floor: `Floor 4`
  - Line Name: `Line 29 - Ashulia Knit Expansion`
  - Standard Operators: `44`, Sewing Machines: `42`
* **Findings:**
  - ✅ **Database Save:** MariaDB table `tabProduction Line` contains the new row `L-29` (`facility_unit: PAL-ASH`, `active: 1`).
  - ❌ **UI Problem:** Top badge stays stuck at `PAL-ASH · 28 Lines` and the table ends at `L-28`.
  - **Root Cause:** In `planning_api.py` (`get_line_loading_board`), the `else` block for custom lines omitted `"facility_unit"`. The subsequent filter `[l for l in db_lines if l.get("facility_unit") == facility_unit]` drops `L-29` because `facility_unit` evaluates to `None`.

---

### Test Case 4: Form 4 — Master Production Schedule (`+ New MPS Schedule` Modal)
* **Screen:** `/planning?view=mps`
* **DocType:** `Master Production Schedule` (`tabMaster Production Schedule`)
* **Input Data:**
  - MPS Code: `MPS-2026-AW-W3111`
  - Period: `2026-09-21` to `2026-11-15`
  - Production Facility: `PAL-ASH`, Run Rate: `11,500 pcs/day`, Total Qty: `320,000 pcs`
* **Findings:**
  - ✅ **Database Save:** MariaDB table `tabMaster Production Schedule` successfully created `MPS-2026-00130`.
  - ❌ **UI Problem:** The "Style & Line Allocation Matrix" table continues showing only the 5 default dummy rows (`MPS-26-W37-01` to `MPS-26-W37-05`).
  - **Root Cause:** Backend API `get_master_production_schedule()` queries MariaDB but returns records under an unmapped key (`db_mps_records`), leaving the main `"rows"` array hardcoded to the 5 initial entries.

---

### Test Case 5: Form 5 — Line Capacity Specification (`+ Add Line Capacity` Modal)
* **Screen:** `/planning?view=capacity`
* **DocType:** `Line Capacity` (`tabLine Capacity`)
* **Input Data:**
  - Production Line: `L-03 (Floor 1)`
  - Operators: `44`, Daily Hours: `10.0`, Target Efficiency: `74%`, Standard SMV: `14.5`
  - Expected Capacity: $\frac{44 \times 10.0 \times 60}{14.5} \times 74\% = 2,246 \text{ pcs/day}$
* **Findings:**
  - ✅ **Database Save:** Record successfully inserted into MariaDB `tabLine Capacity`.
  - ❌ **UI Problem:** Row `L-03` in the table remained at 72% efficiency and 1,267 pcs/day. The top KPI card "Effective Daily Capacity" remained static at 31,108 pcs/day.
  - **Root Cause:** In the live backend Docker container, `get_capacity_model()` does not query `tabLine Capacity`. It computes output strictly using default assumptions (10h, 15 SMV) from `board.lines`.

---

### Test Case 6: Form 6 — S&OP Reconciliation Cycle (`+ New S&OP Cycle` Modal)
* **Screen:** `/planning?view=sop`
* **DocType:** `SOP Cycle` (`tabSOP Cycle`)
* **Input Data Tested:**
  - Cycle Name: `SOP-2026-TEST`
  - Month: `2026-10`
  - Unconstrained Demand: `2,500,000 pcs`
  - Constrained Capacity: `2,100,000 pcs`
  - Subcontract Qty: `400,000 pcs`
  - Status: `Open`
* **Findings:**
  - ✅ **Database Save:** Record `SOP-00136` successfully created in MariaDB `tabSOP Cycle`.
  - ✅ **UI Dynamic Reflection:** The page updated in real time immediately upon saving:
    1. Header title became `Sales & Operations Planning (S&OP) — Cycle SOP-2026-TEST`
    2. Subtitle updated to `Month: 2026-10 · Status: Status: Open`
    3. Unconstrained Demand updated to `25,00,000 pcs`
    4. Factory Constrained Capacity updated to `21,00,000 pcs`
    5. Capacity Deficit (Gap) automatically recalculated to `-4,00,000 pcs`
    6. Disclosed Subcontracting updated to `4,00,000 pcs`
  - **Note on Bottom Table:** The table below ("S&OP 4-Step Consensus Signoff Workflow") is a 4-stage governance approval workflow (Demand Review $\to$ Supply Review $\to$ Pre-S&OP $\to$ Executive Signoff), completed by clicking the `Approve Cycle (COO Signoff)` button. It is not an S&OP Cycle history list.

---

## 3. Summary of Documented Issues & Root Causes

All technical issues found during this session have been documented in detail in [`Issue/demand_issue.md`](file:///Users/laptopparadise/Documents/Textile/Issue/demand_issue.md):

1. **Issue 1:** Demand Ledger KPI Cards are hardcoded strings and EDI poll counter does not persist on browser reload.
2. **Issue 2:** Demand Ledger table does not fetch from MariaDB `tabSales Order`, ignoring newly created Sales Orders (e.g. `PO-44922`).
3. **Issue 3:** Custom Production Lines (e.g. `L-29`) are dropped by `get_line_loading_board()` backend filter due to missing `facility_unit` key in the dictionary.
4. **Issue 4:** Newly created MPS schedules (`MPS-2026-AW-W3111`) are not merged into the API `"rows"` array in `get_master_production_schedule()`.
5. **Issue 5:** Live backend `get_capacity_model()` does not query `tabLine Capacity`, keeping the capacity table and KPI cards static.

---

## 4. Action Items & Next Steps for Engineering

1. **Backend (`planning_api.py`):**
   - Add `"facility_unit": rl.facility_unit or facility_unit or "PAL-ASH"` to the `else` branch of `get_line_loading_board()`.
   - In `get_master_production_schedule()`, merge `persisted_rows` into `all_mps_rows` and return as `"rows"`.
   - Update `get_capacity_model()` inside the Docker container to overlay active records from `tabLine Capacity`.
2. **Frontend (`m08-submodules.tsx` & `page.tsx`):**
   - Connect Demand Ledger table to `/api/commercial?action=orders` so real sales orders appear in the ingestion book.
   - (Optional Enhancement) Add a collapsible "S&OP Cycle Master History" table to `view=sop` if users want to browse previous cycles alongside the active signoff workflow.

---
*Report generated and verified on live local development environment.*
