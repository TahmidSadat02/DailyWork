# GTex ERP Master Testing To-Do List — 15 Oct 2026
## Modules: Demand & Planning (M08-M09) · Quality & Lab (M22-M23) · Textile Manufacturing (M14-M16)

**Project:** Garments & Textile Industry ERP (GTex ERP)  
**Author / QA Tester:** Sadat (Tahmid Sadat)  
**Date:** 15 October 2026  
**File:** `todos and reports/sadat_todo_15oct26.md`  
**Reference Report:** `todos and reports/sadat_report_15oct26.md`  
**User Manual Sources:**  
- `usermanual by module/demand capacity and scheduling user manual/` (M08, M09)  
- `usermanual by module/quality management and laboratory user manual/` (M22, M23)  
- `usermanual by module/textile manufacturing user manual/` (M14, M15, M16)  

**Live Testing Environments:**  
- **Frontend Cockpits:**  
  - Planning & Scheduling: [`http://localhost:3000/planning`](http://localhost:3000/planning)  
  - Quality & Inspection: [`http://localhost:3000/quality`](http://localhost:3000/quality)  
  - Textile Manufacturing: [`http://localhost:3000/textile`](http://localhost:3000/textile)  
- **Frappe Desk Backend:** [`http://localhost:8090/desk`](http://localhost:8090/desk) or [`http://localhost:8081/app`](http://localhost:8081/app) (Site: `dev.localhost`, User: `Administrator`, Pass: `admin`)  

---

## 1. Testing Objectives & Protocol Standards

Today's testing roadmap executes exhaustive end-to-end verification across **3 core enterprise modules**:
1. **Demand Forecasting, Factory Capacity & Scheduling (M08 & M09):** Validate line loading Gantt schedules, automated IE rebalancing proposals, overtime planning, buyer demand forecasting, MPS rolling buckets, and S&OP demand-supply netting.
2. **Quality Management & Laboratory Information Management (M22 & M23):** Validate real-time shopfloor inspection queues, ANSI/ASQ Z1.4 sampling calculator, closed-vocabulary defect taxonomy, ASTM D5430 fabric 4-point rating, PRD-F156 anti-gaming DHU divergence alerts, LIMS physical/chemical test compliance, and metrology calibration logs.
3. **Textile Manufacturing (M14, M15 & M16):** Validate live HT dyeing machine telemetry, one-click batch releases, spinning mill spindle RPM tracking, knitting circular machines greige output, stenter heat-setting logs, barcoded roll inventories, and colour lab spectrophotometer ΔE recipe liquor calculators.
4. **Bi-Directional Synchronization & Persistence:** Verify that frontend submissions trigger Next.js proxy endpoints (`/api/planning`, `/api/quality`, `/api/textile`), commit directly into MariaDB DocTypes via Frappe Python APIs, and refresh across UI views with zero data loss.

---

## 2. PART 1: Demand Forecasting, Capacity & Scheduling (M08 & M09)

### Phase 1.1: Line Loading Board & Multi-Line Gantt Scheduler (`?view=board`)
- [ ] **1.1.1 Header & Live Connection Check**
  - [ ] Open [`http://localhost:3000/planning?view=board`](http://localhost:3000/planning?view=board).
  - [ ] Verify facility header: `"Line loading board — PAL-ASH"` (Ashulia Knitwear Campus).
  - [ ] Confirm green pulsating status chip: `"● Live Frappe MariaDB"`.
  - [ ] Verify sub-header text: `28 lines · week 37 · efficiency-weighted capacity`.
- [ ] **1.1.2 Multi-Line Gantt Timeline Bar Verification**
  - [ ] Inspect Gantt schedule card for Week 37 (Mon 07 to Sat 12).
  - [ ] Verify line bars: `L-03` (`KD-FH115 · 1,612 pcs`), `L-07` (`KD-SH240 · 912 pcs` flagged bottleneck in red), and idle lines (`L-16`).
  - [ ] Click `"Open Gantt"` button in command bar; verify interactive full-screen Gantt modal opens and closes cleanly.
- [ ] **1.1.3 Live Line Loading Table & Totals Computation**
  - [ ] Confirm table columns: *Line*, *Floor*, *PO / Style*, *Buyer*, *Operators*, *Target/hr*, *Produced*, *Eff %*, *WIP (hrs)*, *Status*.
  - [ ] Select row checkboxes individually (`L-01`, `L-03`, `L-07`); confirm footer summary dynamically updates operator headcounts, hourly targets, and total pieces produced.
- [ ] **1.1.4 IE Auto-Balance Engine & Overtime Actions**
  - [ ] Verify amber alert banner: `"2 lines below 60% efficiency (L-07, L-14). IE proposes operator swap at op 14"`.
  - [ ] Click `"Review proposal"` or `"🪄 Auto-balance"` button.
  - [ ] Inspect bottleneck remedy modal (`L-07 Op 14 Waistband attachment`); click `"Apply Proposal Now"`.
  - [ ] Confirm toast notification and verify line efficiency increases from 58% to 68%.
  - [ ] Click `"⏰ Overtime planner"` button; review overtime allocation dialog (BLA compliance check); click `"Approve Overtime (+6h)"`.
- [ ] **1.1.5 Load Plan Publishing & Horizon Switching**
  - [ ] Click `"✓ Publish load plan"`; confirm version increment and publish event sent to Frappe API.
  - [ ] Click `"🔄 Recalculate"`; confirm instant refresh from MariaDB without page reload.
  - [ ] Toggle horizon pills: `[Day]`, `[Week]`, `[Month]`, `[Season]`. Verify time-scale recalculation.
  - [ ] Toggle Facility Unit dropdown from `PAL-ASH` to `PAL-CTG` (Chittagong Denim Campus); verify lines re-filter to coastal denim facilities.

---

### Phase 1.2: Demand Forecasting & Order Book Ledger (`?view=demand`)
- [ ] **1.2.1 Tab Navigation & KPI Metric Cards**
  - [ ] Click `Demand Forecasting` tab or navigate to [`http://localhost:3000/planning?view=demand`](http://localhost:3000/planning?view=demand).
  - [ ] Verify header: `"Demand Forecasting & Order Book Ledger"`.
  - [ ] Verify top 3 KPI cards:
    - [ ] *Consensus Demand:* 1.85M pcs
    - [ ] *Confirmed Orders:* 1.42M pcs (Firm Buyer POs)
    - [ ] *Forecast Accuracy (MAPE):* 94.2% (Historical Backtest)
- [ ] **1.2.2 Buyer Forecast Matrix & Allocation Action**
  - [ ] Inspect forecast matrix table (Buyers: H&M, Zara/Inditex, PVH, Target, Gap, Uniqlo).
  - [ ] Verify columns: *Forecast ID*, *Buyer*, *Season*, *Style Category*, *Projected Units*, *Confirmed POs*, *Variance %*, *Actions*.
  - [ ] Click `"Allocate"` on row `FC-2026-0091`; confirm allocation drawer links units to factory production lines.
- [ ] **1.2.3 '+ New Forecast' Modal Submission (Live MariaDB Persistence)**
  - [ ] Click `"+ New Forecast"` button in top command header.
  - [ ] Fill form modal:
    - [ ] Buyer: `Uniqlo`
    - [ ] Season: `Spring/Summer 2027`
    - [ ] Style: `Men Airism Crewneck Tee`
    - [ ] Projected Units: `150,000`
    - [ ] Target FOB: `$4.85`
  - [ ] Click `"Save & Commit to Frappe"`.
  - [ ] Verify record is created in MariaDB table `tabDemand Forecast` and prepended to the table view.

---

### Phase 1.3: Master Production Schedule MPS (`?view=mps`)
- [ ] **1.3.1 8-Week Time-Phased Matrix & Planning Fences**
  - [ ] Navigate to [`http://localhost:3000/planning?view=mps`](http://localhost:3000/planning?view=mps).
  - [ ] Verify weekly planning horizon (Weeks W38 through W45).
  - [ ] Verify visual vertical planning fence:
    - [ ] *Frozen Zone (Weeks 1–3):* Locked for production; no auto-rescheduling allowed.
    - [ ] *Slushy / Liquid Zone (Weeks 4–8):* Open for capacity smoothing and forecast adjustments.
- [ ] **1.3.2 Capacity Utilization & Overload Alerts**
  - [ ] Verify weekly load bars:
    - [ ] Weeks W38–W40: *Optimal (88%–94%)* (Green)
    - [ ] Week W41: *Overloaded (106%)* (Red warning badge)
  - [ ] Test `"Smooth Load"` button to shift 12,000 units from W41 into under-utilized W42.

---

### Phase 1.4: Factory Capacity Model (`?view=capacity`)
- [ ] **1.4.1 Machine & Workstation Inventory**
  - [ ] Navigate to [`http://localhost:3000/planning?view=capacity`](http://localhost:3000/planning?view=capacity).
  - [ ] Verify factory totals: 1,240 Total Sewing Machines, 88 Cutting Tables, 24 Finishing Tunnels.
  - [ ] Inspect capacity calculation formula card:
    $$\text{Daily Capacity (pcs)} = \frac{\text{Operators} \times \text{Hours} \times 60 \times \text{Efficiency \%}}{\text{Target SMV}}$$
- [ ] **1.4.2 Ramp-Up Learning Curve Simulator**
  - [ ] Navigate to subview [`http://localhost:3000/planning?view=capacity&subview=ramp-curve`](http://localhost:3000/planning?view=capacity&subview=ramp-curve).
  - [ ] Adjust style complexity slider from *Basic Tee (12 SMV)* to *Fleece Jacket (28 SMV)*.
  - [ ] Verify Day 1 to Day 5 learning ramp curve (35% $\rightarrow$ 50% $\rightarrow$ 62% $\rightarrow$ 72% $\rightarrow$ 80%).

---

### Phase 1.5: S&OP Review & Demand-Supply Reconciliation (`?view=sop`)
- [ ] **1.5.1 Executive Consensus Dashboard**
  - [ ] Navigate to [`http://localhost:3000/planning?view=sop`](http://localhost:3000/planning?view=sop).
  - [ ] Inspect monthly reconciliation table (Unconstrained Demand vs Factory Constrained Capacity).
  - [ ] Review capacity deficit balancing via approved tier-1 subcontractors.
- [ ] **1.5.2 4-Step Executive Sign-Off Flow**
  - [ ] Verify sign-off audit stamps: Sales Director, Production VP, Supply Chain Lead, Finance Director.
  - [ ] Click `"Sign Off S&OP Cycle"`; confirm timestamp and cryptographic audit token generation.

---

## 3. PART 2: Quality Management & Laboratory (M22 & M23)

### Phase 2.1: Live Quality Console & Inspection Queue (`http://localhost:3000/quality`)
- [ ] **2.1.1 Overview KPIs & Dynamic Period Filters**
  - [ ] Open [`http://localhost:3000/quality`](http://localhost:3000/quality).
  - [ ] Verify 5 KPI cards: *First Time Through (FTT %)*, *Defects Per Hundred Units (DHU)*, *Active Inspections*, *Critical Defects*, *Lab Pass Rate*.
  - [ ] Toggle period selector ribbon: `[Today]`, `[Week]`, `[Month]`, `[Quarter]`.
  - [ ] Confirm dynamic metric recalculation (e.g., Week view shows 32 inspections, 98.1% pass rate).
- [ ] **2.1.2 Live Inspection Queue Table**
  - [ ] Verify table headers: *Inspection ID*, *Buyer*, *Style*, *Lot Size*, *Sample Size*, *Stage*, *AQL Standard*, *Defects Found*, *Result*, *Actions*.
  - [ ] Locate failing lot `IN-26-1186` (Result: `REJECT` in red chip).
- [ ] **2.1.3 Pareto Donut SVG & Failing Lot Detail**
  - [ ] Inspect interactive Defect Pareto Donut chart (Broken Stitch, Skipped Stitch, Shade Variation, Oil Spot).
  - [ ] Hover over slices; confirm tooltip displays defect frequency % and count.
  - [ ] Inspect Failing Lot Card (`IN-26-1186`): verify critical defect alert and inspector name.
- [ ] **2.1.4 Interactive Modals Execution**
  - [ ] **New Inspection Modal (`#dlg-insp`):**
    - [ ] Click `"New inspection"` in the command bar.
    - [ ] Fill: Buyer `Uniqlo`, Style `KD-FH115`, Lot Size `5000`, AQL `Normal Level II`.
    - [ ] Submit; verify row `IN-26-1188` prepends to the table and persists to MariaDB `tabFinal Inspection`.
  - [ ] **Open CAPA Modal:**
    - [ ] Click `"Open CAPA"` on failing row `IN-26-1186`.
    - [ ] Inspect 5-Why root cause analysis inputs; submit corrective action; verify badge updates.
  - [ ] **Buyer Protocols Modal:**
    - [ ] Click `"Buyer protocols"`; verify AQL tolerances for Uniqlo, Gap, Primark, M&S, C&A, H&M.
  - [ ] **View Photos Modal:**
    - [ ] Click `"View photos"` on `IN-26-1186`; verify high-res macro captures with component tags.

---

### Phase 2.2: AQL Sampling & ANSI/ASQ Z1.4 Simulator (`?view=sampling&subview=aql-calculator`)
- [ ] **2.2.1 Interactive Lot Size Slider**
  - [ ] Navigate to [`http://localhost:3000/quality?view=sampling&subview=aql-calculator`](http://localhost:3000/quality?view=sampling&subview=aql-calculator).
  - [ ] Drag lot size slider across thresholds: 500 $\rightarrow$ 3,200 $\rightarrow$ 10,000 $\rightarrow$ 35,000 pcs.
  - [ ] Verify sample size code letter transitions (e.g. Code J = 80 samples, Code K = 125, Code L = 200).
- [ ] **2.2.2 Acceptance / Rejection Number Determination**
  - [ ] Verify dual-tier AQL limits for General Inspection Level II:
    - [ ] Critical (AQL 0.0): Accept = 0, Reject = 1
    - [ ] Major (AQL 1.5 / 2.5): Dynamic Accept / Reject boundary
    - [ ] Minor (AQL 4.0): Dynamic Accept / Reject boundary

---

### Phase 2.3: Closed-Vocabulary Defect Taxonomy Master (`?view=defects&subview=defect-taxonomy`)
- [ ] **2.3.1 Defect Classification Catalog**
  - [ ] Navigate to [`http://localhost:3000/quality?view=defects&subview=defect-taxonomy`](http://localhost:3000/quality?view=defects&subview=defect-taxonomy).
  - [ ] Verify 6 operational categories: *Cutting*, *Stitching*, *Wash*, *Fabric*, *Finishing*, *Packaging*.
  - [ ] Confirm closed-vocabulary codes: `ST-01` (Broken Stitch), `ST-04` (Puckering), `WA-02` (Bleach Spot).
- [ ] **2.3.2 Garment Part Anatomy Location Mapping**
  - [ ] Verify garment zones: Collar, Armhole, Placket, Inseam, Hem, Pocket, Side Seam.

---

### Phase 2.4: ASTM D5430 Fabric 4-Point System (`?view=fabric&subview=four-point-system`)
- [ ] **2.4.1 Roll Defect Point Calculations**
  - [ ] Navigate to [`http://localhost:3000/quality?view=fabric&subview=four-point-system`](http://localhost:3000/quality?view=fabric&subview=four-point-system).
  - [ ] Verify 4-Point formula:
    $$\text{Points per } 100 \text{ sq. yd} = \frac{\text{Total Defect Points} \times 3,600}{\text{Inspected Length (yd)} \times \text{Fabric Width (in)}}$$
  - [ ] Verify penalty classifications: 1 point ($<3''$), 2 points ($3''-6''$), 3 points ($6''-9''$), 4 points ($>9''$ or holes).
  - [ ] Confirm Grade A ($\le 28$ pts), Grade B ($29-40$ pts), and Reject ($>40$ pts) thresholds.

---

### Phase 2.5: PRD-F156 Anti-Gaming & Cost of Poor Quality (COPQ)
- [ ] **2.5.1 Anti-Gaming Reconciliation Engine (`?view=analytics&subview=anti-gaming`)**
  - [ ] Navigate to [`http://localhost:3000/quality?view=analytics&subview=anti-gaming`](http://localhost:3000/quality?view=analytics&subview=anti-gaming).
  - [ ] Inspect cross-station reconciliation: Inline Inspection DHU vs End-of-Line Audit vs Buyer Final Audit.
  - [ ] Confirm Sewing Line 4 shows **`BREACH TRIGGERED`** red alert badge due to $+6.6\%$ divergence ($>5\%$ PRD-F156 threshold).
- [ ] **2.5.2 COPQ 4-Quadrant Explorer (`?view=qms&subview=copq`)**
  - [ ] Navigate to [`http://localhost:3000/quality?view=qms&subview=copq`](http://localhost:3000/quality?view=qms&subview=copq).
  - [ ] Review financial allocations:
    - [ ] Prevention Costs (Operator training, machine maintenance)
    - [ ] Appraisal Costs (Lab tests, inline QCs)
    - [ ] Internal Failure Costs (Rework, fabric scrap)
    - [ ] External Failure Costs (Chargebacks, late air shipments)

---

### Phase 2.6: LIMS Laboratory Testing & Instrument Calibration
- [ ] **2.6.1 LIMS Testing Workbench (`?view=testing&subview=lims-bench`)**
  - [ ] Navigate to [`http://localhost:3000/quality?view=testing&subview=lims-bench`](http://localhost:3000/quality?view=testing&subview=lims-bench).
  - [ ] Physical Tests: verify Fabric GSM (ISO 3801), Tear Strength (ASTM D1424), Pilling (ISO 12945-2).
  - [ ] Chemical RSL Tests: verify Formaldehyde ($<20$ ppm OEKO-TEX), pH (5.5–7.0), Azo Dyes (Non-detectable).
- [ ] **2.6.2 Metrology Instrument Calibration (`?view=instruments&subview=calibration`)**
  - [ ] Navigate to [`http://localhost:3000/quality?view=instruments&subview=calibration`](http://localhost:3000/quality?view=instruments&subview=calibration).
  - [ ] Locate instrument `INST-GSM-04` with near-expiry health warning.
  - [ ] Click `"Calibrate"` button.
  - [ ] Verify record created in MariaDB `tabCalibration Record` and expiry reset to $+365$ days.

---

## 4. PART 3: Textile Manufacturing (M14, M15, M16)

### Phase 3.1: Wet Processing & Dyeing Batch Console (`http://localhost:3000/textile?view=dyeing`)
- [ ] **3.1.1 Active Dye Lots & SCADA Vessel Telemetry**
  - [ ] Open [`http://localhost:3000/textile?view=dyeing`](http://localhost:3000/textile?view=dyeing).
  - [ ] Verify **"Active Dye Lots on Shop Floor (MariaDB `tabDye Lot`)"** table:
    - [ ] Check batch codes: `DY-4471`, `DY-4475`, `DY-4482`.
    - [ ] Verify attributes: Fabric, Weight (kg), Machine, Liquor Ratio, Stage, Status.
  - [ ] Verify **"Live HT Jet Dyeing Machines"** grid:
    - [ ] Check 14 HT machines (`HT-01` to `HT-14`).
    - [ ] Confirm real-time vessel temperature (°C), steam pressure (bar), and active color swatch.
- [ ] **3.1.2 Command Header Actions**
  - [ ] **Release Batch (`DY-4471`):**
    - [ ] Click blue button `"Release DY-4471"` in command header.
    - [ ] Confirm toast: *"Batch DY-4471 released to finishing — lab approval recorded, recipe locked"*.
    - [ ] Verify stage updates to `Finishing` and RFT status becomes `RFT Pass`.
  - [ ] **New Dye Batch Modal:**
    - [ ] Click `"+ New Dye Batch"` button.
    - [ ] Fill form: Batch No `DY-5002`, Recipe `REC-BLACK-01`, Weight `1,400 kg`, Machine `HT-05`.
    - [ ] Click submit; confirm row is inserted into MariaDB `tabDye Lot` and appears immediately in table.
  - [ ] **Schedule Machine Modal:**
    - [ ] Click `"Schedule Machine"`; assign batch `DY-5002` to `HT-08`; set time `15:30`; confirm schedule.
  - [ ] **Open Re-Dye Job Modal:**
    - [ ] Click `"Open Re-Dye Job"`; enter off-shade batch `DY-4485`, reason *"ΔE 1.4 under TL84"*; submit to MariaDB.
  - [ ] **Approve Lab Shade:**
    - [ ] Under *"Lab — Pending Shade Approvals"*, click `"Approve"` for `DY-4471`; verify ΔE 0.7 Pass.

---

### Phase 3.2: Spinning Mill Floor & Spindle Telemetry (`http://localhost:3000/textile?view=spinning`)
- [ ] **3.2.1 Spinning Mill Floor Overview**
  - [ ] Click `Spinning Mill (M14)` tab or navigate to [`http://localhost:3000/textile?view=spinning`](http://localhost:3000/textile?view=spinning).
  - [ ] Verify KPI Cards:
    - [ ] *Total Spindles:* 8,064 (Running: 7,056)
    - [ ] *Daily Yarn Output:* 23,350 kg
    - [ ] *Average Yarn Count:* 28/1 Ne Combed
    - [ ] *Pneumafil Waste:* 1.42% (Target: $<1.8\%$)
- [ ] **3.2.2 Ringframe Machine Monitoring Table**
  - [ ] Inspect table for Ringframes `RF-01` through `RF-08`.
  - [ ] Verify columns: *Machine No*, *Spinning Type*, *Yarn Count*, *Spindle RPM (e.g. 18,200)*, *Output (kg)*, *Efficiency %*, *Waste %*, *Status*.
  - [ ] Click `"Sync Frappe"` in top-right to pull updated telemetry.

---

### Phase 3.3: Knitting Floor & Greige Rolls (`http://localhost:3000/textile?view=knitting`)
- [ ] **3.3.1 Circular Machine Floor Overview**
  - [ ] Click `Knitting Floor (M15)` tab or navigate to [`http://localhost:3000/textile?view=knitting`](http://localhost:3000/textile?view=knitting).
  - [ ] Verify KPI Cards:
    - [ ] *Active Circular Machines:* 7 active
    - [ ] *Greige Output Today:* 15,180 kg (77 Rolls doffed)
    - [ ] *Knitting Efficiency:* 91.8%
    - [ ] *4-Point 1st Grade:* 98.4%
- [ ] **3.3.2 Machine Status & Doffing Register**
  - [ ] Inspect circular machines `KM-01` through `KM-08`.
  - [ ] Verify attributes: Machine Type (Single Jersey / Rib / Interlock / Fleece), Gauge (24GG/28GG), Diameter (30"/32"), Linked Yarn Lot, GSM, Rolls Knitted, Status badge.

---

### Phase 3.4: Finishing, Stenter & Finished Rolls (`http://localhost:3000/textile?view=finishing`)
- [ ] **3.4.1 Stenter & Compactor Heat-Setting Telemetry**
  - [ ] Click `Finishing & Stenter (M16.7)` tab or navigate to [`http://localhost:3000/textile?view=finishing`](http://localhost:3000/textile?view=finishing).
  - [ ] Inspect Stenter units `ST-01` and `ST-02`: chamber temperatures (185°C), speed (32 m/min), delivery width (68"), and finished GSM.
- [ ] **3.4.2 Inspected Fabric Rolls Table (`tabFabric Roll`)**
  - [ ] Verify table rows for finished rolls ready for cutting floor.
  - [ ] Check roll details: *Roll No*, *Yarn Lot*, *Shade Band*, *Net Weight (kg)*, *Net Length (m)*, *Width (in)*, *GSM*, *QC Status*.

---

### Phase 3.5: Colour Lab & Recipe Library (`http://localhost:3000/textile?view=recipe`)
- [ ] **3.5.1 Dye Recipe Search & Spectrophotometer Pass**
  - [ ] Click `Recipe Library & Lab` tab or navigate to [`http://localhost:3000/textile?view=recipe`](http://localhost:3000/textile?view=recipe).
  - [ ] Type `"Royal"` in search filter; confirm instant filter to `REC-ROYAL-05` with color swatch `#4169E1`.
  - [ ] Verify chemical formulation items (Reactive Blue, Soda Ash, Glauber Salt) and g/kg concentrations.
- [ ] **3.5.2 Recipe Liquor Volume & Dosage Multiplier**
  - [ ] In the top bar, set **Batch Wt (kg)** to `1,200`.
  - [ ] Set **Liquor Ratio (1:X)** to `6`.
  - [ ] Verify total liquor recomputes to **`7,200 Litres`** and chemical dosages automatically recalculate in kg and grams.

---

## 5. PART 4: Automated Testing, Regression Runner & Evidence Capture

- [ ] **4.1 Automated Python Test Suite Execution**
  - [ ] Run Planning suite: `python3 scratch/comprehensive_planning_browser_test.py`
  - [ ] Run Quality suite: `python3 scratch/test_quality_module.py`
  - [ ] Run Backend Docker unit tests:
    ```bash
    docker exec gtex-backend-1 bench --site dev.localhost execute grp_commercial.tests.run_tests.run
    docker exec gtex-backend-1 bench --site dev.localhost execute grp_compliance.tests.run_tests.run
    docker exec gtex-backend-1 bench --site dev.localhost execute grp_textile.tests.run_tests.run
    ```
- [ ] **4.2 Visual Proof Artifacts Synchronization**
  - [ ] Verify all screenshots captured during testing are saved to:
    - [ ] `usermanual by module/demand capacity and scheduling user manual/screenshots/`
    - [ ] `usermanual by module/quality management and laboratory user manual/screenshots/`
    - [ ] `usermanual by module/textile manufacturing user manual/screenshots/`
- [ ] **4.3 Report Compilation**
  - [ ] Reconcile test results into `todos and reports/sadat_report_15oct26.md`.
  - [ ] Commit and push to GitHub repository `TahmidSadat02/DailyWork.git` upon completion.
