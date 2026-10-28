# GTex ERP Master Testing To-Do List — 28 Oct 2026
## Domain 05: Textile Manufacturing & Wet Processing (M14–M17) End-to-End Flow

**Project:** Garments & Textile Industry ERP (GTex ERP)  
**Author / QA Tester:** Sadat (Tahmid Sadat)  
**Date:** 28 October 2026  
**File:** [`todos and reports/sadat_todo_28oct.md`](file:///Users/laptopparadise/Documents/Textile/todos%20and%20reports/sadat_todo_28oct.md)  
**Companion Report File:** [`todos and reports/sadat_report_28oct.md`](file:///Users/laptopparadise/Documents/Textile/todos%20and%20reports/sadat_report_28oct.md)  
**Audited Input Guide:** [`presentation/M14_M17_TEXTILE_MANUFACTURING_SCREENS_INPUT_GUIDE.md`](file:///Users/laptopparadise/Documents/Textile/presentation/M14_M17_TEXTILE_MANUFACTURING_SCREENS_INPUT_GUIDE.md)  
**Story Reference:** [`presentation/GTEX_17_USERS_CREDENTIALS_AND_FULL_STORY_FLOW.md`](file:///Users/laptopparadise/Documents/Textile/presentation/GTEX_17_USERS_CREDENTIALS_AND_FULL_STORY_FLOW.md) (Step 5, 6 & 7 — Giorgio Armani `PO-77190`, Style `MN-PL412`, 50,000 pcs Polo)  
**Issue Tracker Log:** [`Issue/textile_issue.md`](file:///Users/laptopparadise/Documents/Textile/Issue/textile_issue.md) & [`Issue/textile_issue.docx`](file:///Users/laptopparadise/Documents/Textile/Issue/textile_issue.docx)  

---

## 0. Live Environments, Portals & Credentials Reference

### Active Testing Portals:
* **Next.js Frontend Textile Console:** [`http://localhost:3000/textile`](http://localhost:3000/textile)
* **Next.js Frontend Sourcing Console:** [`http://localhost:3000/supply`](http://localhost:3000/supply)
* **Frappe Desk Backend:** [`http://localhost:8081/login`](http://localhost:8081/login) *(or [`http://localhost:8090/desk`](http://localhost:8090/desk))*
* **MariaDB Live Container:** `gtex-mariadb-1` (Site: `dev.localhost`, DB: `_a0f2b0ce3c03d38f`, User: `_a0f2b0ce3c03d38f`, Pass: `vAbsxKBLGpKarvGE`)

### User Roles & Login Credentials:
| Portal & Role | Direct Frontend / Desk URL | User Email / ID | Password | Role Description |
|---|---|---|---|---|
| **Textile Mill GM & Dyeing Master** | [`http://localhost:3000/textile?view=dyeing`](http://localhost:3000/textile?view=dyeing) | `textile.jahangir@gtexgroup.com.bd` | `Gtex@2026` | Md. Jahangir Alam (Full M14–M17 Execution) |
| **Spinning Floor In-charge** | [`http://localhost:3000/textile?view=spinning`](http://localhost:3000/textile?view=spinning) | `textile.jahangir@gtexgroup.com.bd` | `Gtex@2026` | Abdul Alim (30/1 Ne Ringframe Monitoring) |
| **Knitting Production Master** | [`http://localhost:3000/textile?view=knitting`](http://localhost:3000/textile?view=knitting) | `textile.jahangir@gtexgroup.com.bd` | `Gtex@2026` | Dulal Mia (Circular Knitting & Greige Doffing) |
| **Finishing & Stenter Supervisor** | [`http://localhost:3000/textile?view=finishing`](http://localhost:3000/textile?view=finishing) | `textile.jahangir@gtexgroup.com.bd` | `Gtex@2026` | Rafiqul Islam (Stenter & Roll Stock Inventory) |
| **Chief Colorist & Lab In-charge** | [`http://localhost:3000/textile?view=recipe`](http://localhost:3000/textile?view=recipe) | `textile.jahangir@gtexgroup.com.bd` | `Gtex@2026` | Dr. Kabir (Dye Recipe RIP v6 & ΔE Matching) |
| **AOP, Embroidery & Wash Manager**| [`http://localhost:3000/textile?view=wash-print`](http://localhost:3000/textile?view=wash-print) | `textile.jahangir@gtexgroup.com.bd` | `Gtex@2026` | Tarek Hasan (Strike-offs, Tajima & Belly Wash) |
| **Super Admin / Auditor** | [`http://localhost:3000/textile`](http://localhost:3000/textile) | `Administrator` | `admin` | System Auditor & Frappe Administrator |
| **Sourcing & Purchase Officer** | [`http://localhost:3000/supply`](http://localhost:3000/supply) | `sourcing.kabir@gtexgroup.com.bd` | `Gtex@2026` | Kabir Hossain (Fabric & Yarn MRP Bookings) |

---

## 1. Pre-Flight Health Check & Infrastructure Verification

- [ ] **0.1 Frontend Server Check:**
  - [ ] Run `curl -I http://localhost:3000/textile` — verify HTTP 200 OK.
  - [ ] Open [`http://localhost:3000/textile`](http://localhost:3000/textile) in browser. Confirm application loads with zero console crashes.
- [ ] **0.2 Frappe Backend Container Check:**
  - [ ] Run `docker ps` — verify containers `gtex-frontend-1`, `gtex-backend-1`, and `gtex-mariadb-1` are up and running.
  - [ ] Test Frappe REST API health: `curl -I http://localhost:8081` or `:8090`.
- [ ] **0.3 MariaDB Connectivity & Master Seeds Check:**
  - [ ] Run terminal check:
    ```bash
    docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "
    SELECT name, recipe_code, shade_band FROM \`tabDye Recipe\`;
    SELECT count(*) as total_lots FROM \`tabDye Lot\`;
    SELECT count(*) as total_rolls FROM \`tabFabric Roll\`;
    "
    ```
  - [ ] Confirm master recipe `REC-NAVY-01` is seeded in `tabDye Recipe` to prevent `LinkValidationError`.

---

## 2. Proper Flow Step 1: Spinning Mill Floor (M14) & Spindle Monitoring

* **Screen Route:** [`http://localhost:3000/textile?view=spinning`](http://localhost:3000/textile?view=spinning)
* **Frappe Desk Link:** [`http://localhost:8081/app/spinning-log`](http://localhost:8081/app/spinning-log) & [`http://localhost:8081/app/yarn-lot/YL-2026-C30`](http://localhost:8081/app/yarn-lot/YL-2026-C30)
* **MariaDB Target:** `tabSpinning Log` & `tabYarn Lot`

### Checklist:
- [ ] **1.1 Screen Navigation & Header Audit:**
  - [ ] Click the **`Spinning Mill (M14)`** tab on the navigation bar.
  - [ ] Verify header: `"Spinning Mill Floor — PTL-SPIN"`.
  - [ ] Verify sub-header: `"8,064 Spindles · Count Ne Monitoring · Spindle RPM · Pneumafil Waste Tracking"`.
- [ ] **1.2 Top KPI Metric Cards Verification:**
  - [ ] *Total Spindles:* Verify card displays `8,064 Spindles` (8 Ringframes × 1,008 spindles).
  - [ ] *Active Spindles:* Verify running count displays `7,056 Spindles` (7 running, 1 idle/maintenance).
  - [ ] *Daily Output:* Verify output reflects approximately `22,460 kg – 23,350 kg`.
  - [ ] *Average Efficiency:* Verify average ringframe efficiency is `~94.2% – 94.8%`.
  - [ ] *Pneumafil Waste:* Verify pneumafil waste card displays `1.8%` (target strictly under 2.0%).
  - [ ] *Raw Cotton Bale Inventory:* Verify stock of `480 Bales` available.
- [ ] **1.3 Ringframe Machines RF-01 to RF-08 Floor Table Audit:**
  - [ ] Verify **RF-01**: `30/1 Ne Combed 100% Cotton`, 1,008 Spindles, 18,500 RPM, 19.5 TPI, 2,840 kg, `95.2%` Eff, 1.4% Waste, Status: `Running`.
  - [ ] Verify **RF-02**: `30/1 Ne Combed 100% Cotton`, 1,008 Spindles, 18,500 RPM, 2,790 kg, `94.8%` Eff, 1.6% Waste, Status: `Running`.
  - [ ] Verify **RF-03**: `34/1 Ne Super Combed`, 1,008 Spindles, 19,200 RPM, 2,620 kg, Status: `Running`.
  - [ ] Verify **RF-04**: `24/1 Ne Carded Cotton`, 1,008 Spindles, 17,800 RPM, 3,210 kg, Status: `Running`.
  - [ ] Verify **RF-05**: `20/1 Ne Poly/Cotton 60/40`, 1,008 Spindles, 16,500 RPM, 3,450 kg, Status: `Running`.
  - [ ] Verify **RF-06**: `26/1 Ne Slub Effect Yarn`, 1,008 Spindles, 17,000 RPM, 2,980 kg, Status: `Running`.
  - [ ] Verify **RF-07**: `30/1 Ne Combed Cotton`, 1,008 Spindles, 18,500 RPM, 2,810 kg, Status: `Running`.
  - [ ] Verify **RF-08**: 1,008 Spindles, 0 RPM, Status: `Maintenance / Doffing`.
- [ ] **1.4 Raw Fibre Intake Verification:**
  - [ ] Verify raw cotton bale lots: `LOT-COT-USA-2026-08` (US Pima, Mic 4.1, Staple 32.4mm, 140 bales), `LOT-COT-CIS-2026-11` (Uzbek Medium Staple, 220 bales), `LOT-POLY-IND-2026-04` (Virgin Bright, 120 bales).
- [ ] **1.5 Frappe Desk & MariaDB Cross-Verification:**
  - [ ] Open [`http://localhost:8081/app/yarn-lot/YL-2026-C30`](http://localhost:8081/app/yarn-lot/YL-2026-C30). Confirm lot exists for 30/1 Ne Combed yarn.
  - [ ] Terminal query:
    ```bash
    docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "SELECT name, lot_code, yarn_description, count_ne, lot_weight_kg, status FROM \`tabYarn Lot\` WHERE lot_code LIKE '%C30%' OR name LIKE '%C30%';"
    ```

---

## 3. Proper Flow Step 2: Knitting Floor (M15) & Greige Formation

* **Screen Route:** [`http://localhost:3000/textile?view=knitting`](http://localhost:3000/textile?view=knitting)
* **Frappe Desk Link:** [`http://localhost:8081/app/knitting-log`](http://localhost:8081/app/knitting-log)
* **MariaDB Target:** `tabKnitting Log` & `tabFabric Roll`

### Checklist:
- [ ] **2.1 Screen Navigation & Header Audit:**
  - [ ] Click the **`Knitting Floor (M15)`** tab on the navigation bar.
  - [ ] Verify header: `"Knitting & Fabric Formation Floor — PTL-GAZ"`.
  - [ ] Verify sub-header: `"Circular & Flat Collar Knitting · Gauge & Dia Matching · 4-Point Greige Inspection"`.
- [ ] **2.2 Top KPI Metric Cards Verification:**
  - [ ] *Active Machines:* Verify 7 of 8 circular knitting machines running.
  - [ ] *Rolls Doffed Today:* Verify roll counter displays `~77 – 85 Rolls`.
  - [ ] *Greige Output (kg):* Verify total greige weight displays `~15,500 – 16,500 kg`.
  - [ ] *Knitting Efficiency:* Verify average knitting efficiency is `91.8%`.
  - [ ] *4-Point Inspection Pass Rate:* Verify first grade pass rate is `98.4%`.
  - [ ] *Defect Points / 100m²:* Verify defect score is `14.2 pts` (acceptable threshold < 20).
- [ ] **2.3 Circular Knitting Machine Fleet KM-01 to KM-08 Audit:**
  - [ ] Verify **KM-01**: Circular Single Jersey, 28GG · Dia 30", 30/1 Ne Combed, 160 GSM, 12–14 rolls, `2,160–2,730 kg`, Status: `Running`.
  - [ ] Verify **KM-02**: Circular Single Jersey, 28GG · Dia 32", 30/1 Ne Combed, 160 GSM, 13–14 rolls, Status: `Running`.
  - [ ] Verify **KM-03**: Interlock Double Jersey, 24GG · Dia 30", 24/1 Ne Carded, 210 GSM, Status: `Running`.
  - [ ] Verify **KM-04**: Circular Rib 1x1, 18GG · Dia 30", 20/1 Ne + 40D Lycra, 220 GSM, Status: `Running`.
  - [ ] Verify **KM-05**: 3-Thread Fleece, 20GG · Dia 34", 280–320 GSM, Status: `Running`.
  - [ ] Verify **KM-06**: Circular Pique Polo, 24GG · Dia 34", 30/1 Ne, 220 GSM, Status: `Running`.
  - [ ] Verify **KM-07**: Circular Single Jersey, 24GG · Dia 30", 24/1 Ne CVC Melange, 170 GSM, Status: `Running`.
  - [ ] Verify **KM-08**: Collar Knit / Creel Loading / Changeover.
- [ ] **2.4 Greige Inventory Allocation Check:**
  - [ ] Verify greige inventory table links rolls to downstream dye lots:
    - [ ] `100% Cotton Single Jersey 160 GSM (Dia 30")` $\to$ Allocated to Dye Lot `DY-4471`.
    - [ ] `Cotton / Elastane 1x1 Rib 220 GSM (Dia 30")` $\to$ Allocated to Dye Lot `DY-4476`.
    - [ ] `French Terry Fleece 320 GSM (Dia 34")` $\to$ Allocated to Dye Lot `DY-4482`.
- [ ] **2.5 Frappe Desk & MariaDB Cross-Verification:**
  - [ ] Open [`http://localhost:8081/app/knitting-log`](http://localhost:8081/app/knitting-log). Verify logs are accessible.

---

## 4. Proper Flow Step 3: Colour Lab & Recipe Library RIP v6

* **Screen Route:** [`http://localhost:3000/textile?view=recipe`](http://localhost:3000/textile?view=recipe)
* **Frappe Desk Link:** [`http://localhost:8081/app/dye-recipe`](http://localhost:8081/app/dye-recipe) & [`http://localhost:8081/app/dye-recipe/REC-NAVY-01`](http://localhost:8081/app/dye-recipe/REC-NAVY-01)
* **MariaDB Target:** `tabDye Recipe` & `tabDye Recipe Item` & `tabLab Dip Submission`

### Checklist:
- [ ] **3.1 Screen Navigation & Header Audit:**
  - [ ] Click the **`Recipe Library & Lab`** tab on the navigation bar.
  - [ ] Verify header: `"Colour Lab & Dye Recipe Library (RIP)"`.
  - [ ] Verify sub-header: `"Reactive & Disperse Dye Formulations · Grams/Kg Dosages · ZDHC MRSL Gateways"`.
- [ ] **3.2 Buyer Lab Dip Approval Check (Giorgio Armani PO-77190):**
  - [ ] Verify approved shade record for style `MN-PL412`:
    - [ ] Pantone Shade: `19-4010 TPX Total Eclipse Navy`.
    - [ ] Submission Code: `LAB-DIP-2026-0001` (or matching Navy lab dip).
    - [ ] Decision: `Approved (Option B) — ΔE 0.42 under D65 / TL84 light`.
- [ ] **3.3 Interactive Dynamic Recipe Dosing Calculator Testing:**
  - [ ] In the calculator inputs card, enter the exact test values:
    - [ ] **Filter by Shade or Code:** Type `REC-NAVY-01` or `Navy`.
    - [ ] **Batch Weight (kg):** Enter `1000`.
    - [ ] **Liquor Ratio (1:X):** Enter `6` (Ratio 1:6).
  - [ ] **Verify Dynamic Mathematical Calculations:**
    - [ ] Confirm **Total Liquor Volume** dynamically computes to **`6,000 Litres`** ($1,000 \times 6$).
    - [ ] Confirm **Reactive Navy RGB** (Dye @ 28.4 g/kg) computes to **`28.40 kg`**.
    - [ ] Confirm **Reactive Blue BF** (Dye @ 4.2 g/kg) computes to **`4.20 kg`**.
    - [ ] Confirm **Reactive Red 3BS** (Dye @ 1.8 g/kg) computes to **`1.80 kg`**.
    - [ ] Confirm **Glauber Salt (Electrolyte)** (@ 60.0 g/kg) computes to **`60.00 kg`**.
    - [ ] Confirm **Soda Ash (Fixation)** (@ 18.0 g/kg) computes to **`18.00 kg`**.
    - [ ] Confirm **Sequestering Agent** (@ 1.0 g/kg) computes to **`1.00 kg`**.
    - [ ] Confirm **Total Formulation Dosing** shows **`62.40 kg`** total auxiliaries and dyestuff.
  - [ ] Test scaling: Change Batch Weight to `1200` kg — verify Total Liquor Volume updates to `7,200 Litres` and Reactive Navy RGB scales to `34.08 kg`.
- [ ] **3.4 Frappe Desk & MariaDB Master Recipe Verification:**
  - [ ] Open [`http://localhost:8081/app/dye-recipe/REC-NAVY-01`](http://localhost:8081/app/dye-recipe/REC-NAVY-01). Confirm recipe is active with Navy shade band.
  - [ ] Terminal query:
    ```bash
    docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "
    SELECT name, recipe_code, shade_band, total_g_per_kg, is_active FROM \`tabDye Recipe\` WHERE name = 'REC-NAVY-01';
    SELECT parent, item_code, concentration_g_per_kg, step_order FROM \`tabDye Recipe Item\` WHERE parent = 'REC-NAVY-01';
    "
    ```

---

## 5. Proper Flow Step 4: Wet Processing & Jet Dyeing Console (M16)

* **Screen Route:** [`http://localhost:3000/textile?view=dyeing`](http://localhost:3000/textile?view=dyeing)
* **Frappe Desk Link:** [`http://localhost:8081/app/dye-lot`](http://localhost:8081/app/dye-lot)
* **MariaDB Target:** `tabDye Lot`

### Checklist:
- [ ] **4.1 Screen Navigation & Live KPIs Verification:**
  - [ ] Click the **`Wet Processing / Dyeing (M16)`** tab (or open [`http://localhost:3000/textile?view=dyeing`](http://localhost:3000/textile?view=dyeing)).
  - [ ] Verify header: `"Dyeing Batch Console — PTL-GAZ"`.
  - [ ] Verify live KPIs:
    - [ ] *RFT — Right-First-Time:* `86.4%` (+2.1 pts MoM · target 88%).
    - [ ] *Re-Dyes in Queue:* `1` (cost ৳ 3.2 L).
    - [ ] *Batches in Process:* `6` (7,170 kg on floor).
    - [ ] *Chemical Stock-outs (7d):* `2` (Reactive Blue · Acetic Acid).
    - [ ] *Water per kg (ETP):* `68 L` (ZDHC Gateway compliant).
- [ ] **4.2 14 HT Jet Dyeing Machines Live Telemetry Fleet Audit:**
  - [ ] Verify HT-01 to HT-14 telemetry cards (Capacities 500 kg to 1,200 kg, temp gauges up to 130°C, bar pressure up to 4.8 bar).
  - [ ] Confirm machine color chips match active dye shades (Royal Blue `#4169E1`, Navy `#1B2A4A`, Jet Black `#111111`, Olive `#556B2F`).

---

### 4.3 Form 1 Execution: `+ New Dye Batch` Modal Creation
- [ ] **4.3.1 Modal Trigger & Form Display:**
  - [ ] In the Dyeing Console command bar, click the **`+ New Dye Batch`** button.
  - [ ] Verify modal appears with title: `"Create New Dye Lot Batch in Frappe DB"`.
- [ ] **4.3.2 Input Field-by-Field Entry (Exact Values from Input Guide):**
  - [ ] **Dye Lot Number \*:** Enter `DY-4501` *(or next unique number e.g. `DY-4502`)*
  - [ ] **Dye Recipe \*:** Select `REC-NAVY-01` *(verified seeded in MariaDB)*
  - [ ] **Shade Band \*:** Select `NAVY`
  - [ ] **Fabric Type \*:** Enter `100% Combed Cotton Single Jersey 160 GSM`
  - [ ] **Batch Weight (kg) \*:** Enter `1200`
  - [ ] **Target Machine \*:** Select `HT-03`
  - [ ] **Liquor Ratio:** Confirm read-only value shows `1:6`.
- [ ] **4.3.3 Submission & Feedback Check:**
  - [ ] Click the primary button: **`Create in Frappe DB`**.
  - [ ] Verify green success toast appears:
    `"Dye Batch DY-4501 created in Frappe MariaDB!"` *(or corresponding lot name)*.
  - [ ] Verify modal automatically closes cleanly.
  - [ ] Verify the newly created batch appears immediately at the top of the **Live Shopfloor Dye Batches** table.
- [ ] **4.3.4 MariaDB Persistence Verification (Terminal SQL):**
  - [ ] Run terminal query to confirm row exists in MariaDB:
    ```bash
    docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "
    SELECT name, dye_lot_number, dye_recipe, shade_band, total_batch_kg, dyeing_machine, rft_status, status, creation 
    FROM \`tabDye Lot\` 
    ORDER BY creation DESC LIMIT 3;
    "
    ```
  - [ ] Confirm `dye_recipe` = `'REC-NAVY-01'`, `shade_band` = `'NAVY'`, `total_batch_kg` = `1200.00`, `dyeing_machine` = `'HT-03'`, `status` = `'Batching'`.

---

### 4.4 Form 2 Execution: `Schedule Machine Window` Modal
- [ ] **4.4.1 Modal Trigger:**
  - [ ] Click the **`Schedule Machine`** button in the command bar (or click **`Move`** on a batch row).
  - [ ] Verify modal opens: `"Schedule Machine Window"`.
- [ ] **4.4.2 Input Field Entry (Exact Values from Input Guide):**
  - [ ] **Batch Number \*:** Enter `DY-4471`
  - [ ] **Assign to Machine \*:** Select `HT-08`
  - [ ] **Scheduled Start Window \*:** Enter `14:20`
- [ ] **4.4.3 Submission & Feedback Check:**
  - [ ] Click **`Confirm Schedule`**.
  - [ ] Verify toast appears:
    `"Machine scheduler: HT-08 window 14:20 — batch DY-4471 queued."`
  - [ ] Verify machine indicator for the row updates to `HT-08`.
- [ ] **4.4.4 MariaDB Verification:**
  - [ ] Run terminal query:
    ```bash
    docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "
    SELECT name, dye_lot_number, dyeing_machine, modified 
    FROM \`tabDye Lot\` 
    WHERE dye_lot_number = 'DY-4471' OR name = 'DY-4471';
    "
    ```
  - [ ] Confirm `dyeing_machine` equals `'HT-08'`.

---

### 4.5 Form 3 Execution: `Open Re-Dye Job & Cost Claim` Modal
- [ ] **4.5.1 Modal Trigger:**
  - [ ] Click the **`Open Re-Dye Job`** button in the command bar.
  - [ ] Verify modal opens: `"Open Re-Dye Job & Cost Claim"`.
- [ ] **4.5.2 Input Field Entry (Exact Values from Input Guide):**
  - [ ] **Off-Shade Dye Lot Number \*:** Enter `DY-4485`
  - [ ] **Off-Shade Root Cause / Reason \*:** Enter:
    `Off-shade: ΔE 1.4 exceeds tolerance under TL84 light`
  - [ ] **Supplier / Internal Claim Cost \*:** Enter `৳ 3.2 L`
- [ ] **4.5.3 Submission & Feedback Check:**
  - [ ] Click **`Open Re-Dye in Frappe DB`**.
  - [ ] Verify toast appears:
    `"Re-dye job opened for DY-4485 — cost ৳ 3.2 L, claim drafted to supplier."`
  - [ ] Verify batch `DY-4485` status badge flags as red Re-Dye alert.
- [ ] **4.5.4 MariaDB Verification:**
  - [ ] Run terminal query:
    ```bash
    docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "
    SELECT name, dye_lot_number, rft_status, status 
    FROM \`tabDye Lot\` 
    WHERE dye_lot_number = 'DY-4485';
    "
    ```
  - [ ] Confirm `rft_status` is updated to `'Re-dyed'` and `status` is set to `'Dyeing'`.

---

### 4.6 Primary Action: Shopfloor Release Action (`Release DY-4471`)
- [ ] **4.6.1 Action Trigger:**
  - [ ] In the command bar, locate and click the primary action button: **`Release DY-4471`**.
- [ ] **4.6.2 Feedback & State Transition Check:**
  - [ ] Verify success toast notification:
    `"Batch DY-4471 released to finishing!"`
  - [ ] Verify batch `DY-4471` status transitions to `Finishing & Stenter`.
  - [ ] Verify RFT status updates to `RFT - First Time Pass`.
  - [ ] Verify recipe formulation is locked against further editing.
- [ ] **4.6.3 MariaDB Verification:**
  - [ ] Run terminal query:
    ```bash
    docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "
    SELECT name, dye_lot_number, status, rft_status, modified 
    FROM \`tabDye Lot\` 
    WHERE dye_lot_number = 'DY-4471' OR name = 'DY-4471';
    "
    ```
  - [ ] Confirm `status` = `'Finishing & Stenter'` and `rft_status` = `'RFT - First Time Pass'`.

---

## 6. Proper Flow Step 5: Finishing, Stenter & Compactor (M16.7–M16.9)

* **Screen Route:** [`http://localhost:3000/textile?view=finishing`](http://localhost:3000/textile?view=finishing)
* **Frappe Desk Link:** [`http://localhost:8081/app/fabric-roll`](http://localhost:8081/app/fabric-roll) & [`http://localhost:8081/app/fabric-roll/ROLL-00119`](http://localhost:8081/app/fabric-roll/ROLL-00119)
* **MariaDB Target:** `tabFabric Roll`

### Checklist:
- [ ] **5.1 Screen Navigation & Header Audit:**
  - [ ] Click the **`Finishing & Stenter (M16.7)`** tab on the navigation bar.
  - [ ] Verify header: `"Finishing, Stenter & Compactor Console — PTL-GAZ"`.
  - [ ] Verify sub-header: `"Stenter Heat-Setting · Compactor Dimensional Shrinkage · Finished Roll Stock (M16)"`.
- [ ] **5.2 Stenter Heat-Setting Telemetry Cards:**
  - [ ] Inspect **ST-01 (Monforts 8-Chamber)**: Temp `175°C`, Speed `32 m/min`, Width `64" (162 cm)`, Batch `DY-4471` (Navy Single Jersey), Status `Running`.
  - [ ] Inspect **ST-02 (Bruckner 10-Chamber)**: Temp `165°C`, Speed `28 m/min`, Width `72"`, Batch `DY-4476` (Grey Rib), Status `Running`.
- [ ] **5.3 Finished Fabric Roll Stock Inventory Table Audit (Live MariaDB):**
  - [ ] Confirm table displays live rows fetched directly from MariaDB table `tabFabric Roll`.
  - [ ] Check roll **`ROLL-00119`**:
    - [ ] Roll No: `ROLL-00119` *(Internal barcode: `ROLL-4471-01`)*
    - [ ] Yarn Lot: `YL-2026-C30`
    - [ ] Shade: `NAVY`
    - [ ] Net Weight: `182.5 kg`
    - [ ] Net Length: `620 m`
    - [ ] Width: `64"`
    - [ ] GSM: `160`
    - [ ] QC Status: `QC Pass`
  - [ ] Check roll **`ROLL-00120`**: `178.0 kg`, 605 m, Width 64", 160 GSM, `QC Pass`.
  - [ ] Check roll **`ROLL-00013`**: `175.2 kg`, 920 m, Width 62", 160 GSM, `In Stock`.
  - [ ] Check roll **`ROLL-00014`**: `168.9 kg`, 890 m, Width 62", 160 GSM, `In Stock`.
  - [ ] Check roll **`ROLL-00015`**: `181.0 kg`, 955 m, Width 62", 160 GSM, `In Stock`.
- [ ] **5.4 Frappe Desk & MariaDB Cross-Verification:**
  - [ ] Open [`http://localhost:8081/app/fabric-roll/ROLL-00119`](http://localhost:8081/app/fabric-roll/ROLL-00119). Confirm record is intact.
  - [ ] Run terminal query:
    ```bash
    docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "
    SELECT name, roll_no, yarn_lot, shade_band, net_weight_kg, net_length_m, width_inch, gsm, status 
    FROM \`tabFabric Roll\` 
    ORDER BY creation DESC LIMIT 5;
    "
    ```

---

## 7. Proper Flow Step 6: Garment Wash, Print & Embroidery (M17)

* **Screen Route:** [`http://localhost:3000/textile?view=wash-print`](http://localhost:3000/textile?view=wash-print)
* **Frappe Desk Links:**
  - Print Strike-off: [`http://localhost:8081/app/print-strike-off`](http://localhost:8081/app/print-strike-off)
  - Embroidery Design: [`http://localhost:8081/app/embroidery-design`](http://localhost:8081/app/embroidery-design)
  - Wash Recipe: [`http://localhost:8081/app/wash-recipe`](http://localhost:8081/app/wash-recipe)
* **MariaDB Target:** `tabPrint Strike Off`, `tabEmbroidery Design`, `tabWash Recipe`

### Checklist:
- [ ] **6.1 Screen Navigation & Header Audit:**
  - [ ] Click the **`Wash, Print & Embroidery (M17)`** tab on the navigation bar.
  - [ ] Verify header: `"Garment Wash, Screen Print & Computerized Embroidery (M17)"`.
  - [ ] Verify 3 distinct sub-sections: Screen Printing (M17.1), Tajima Embroidery (M17.2), Industrial Wash (M17.3).
- [ ] **6.2 M17.1 Screen Printing Strike-off & Bulk Production Audit:**
  - [ ] Verify Strike-off **`SO-77190-01`**:
    - [ ] Style: `MN-PL412 Polo` (Armani Group)
    - [ ] Print Technique: `Plastisol + High Density Pigment`
    - [ ] Screens: `4 Screens`
    - [ ] Planned: `50,000 pcs`
    - [ ] Printed: `38,400 pcs`
    - [ ] Buyer Approval: `Approved`
    - [ ] Status: `Bulk Printing`
  - [ ] Verify Strike-off **`SO-88214-02`**: `MN-TS460 Basic Tee` (H&M), Discharge Ink, 6 Screens, `142,000 / 180,000 pcs`, Status: `Bulk Printing`.
  - [ ] Verify Strike-off **`SO-66420-03`**: `DN-SJ330 Denim Jean` (M&S), Flock + Foil, Status: `Lab Dip Test`.
  - [ ] Verify Strike-off **`SO-29614-04`**: `LD-LG388 Legging` (Primark), 8 Screens, 140,000 pcs, Status: `Completed`.
- [ ] **6.3 M17.2 Computerized Embroidery Operation Audit:**
  - [ ] Verify **`EMB-MN-01`**: Armani Shield Crest, `Tajima-01 (20 Heads)`, 8,450 stitches, Madeira Polyneon 40 (3 Shades), Speed: 850 SPM, Output: `1,850 pcs/day`, Status: `Running`.
  - [ ] Verify **`EMB-PL-02`**: H&M Collar Tipping, `Tajima-02 (20 Heads)`, 4,200 stitches, Speed: 900 SPM, Output: `2,600 pcs/day`, Status: `Running`.
  - [ ] Verify **`EMB-DN-03`**: M&S Pocket Signature, `Tajima-03 (20 Heads)`, 3,100 stitches, Metallic Gold Thread, Output: `2,100 pcs/day`, Status: `Running`.
  - [ ] Verify **`EMB-UN-04`**: Uniqlo Chino Brand Tab, `Tajima-04 (20 Heads)`, 5,600 stitches, Output: `1,450 pcs/day`, Status: `Running`.
- [ ] **6.4 M17.3 Industrial Garment Wash Recipe & Machine Audit:**
  - [ ] Verify **`GW-9014`**: Recipe `WSH-ENZ-01`, Bio-Polish Enzyme Wash, Tolon BW-01 (250 kg), 800 pcs, Liquor 1:8, Neutral Cellulase 1.5 g/L, 55 min @ 55°C, Stage: `Hydro-Extraction`.
  - [ ] Verify **`GW-9015`**: Recipe `WSH-ACID-02`, Acid Wash & Neutralize, Tolon BW-02 (300 kg), 650 pcs, Pumice Stones + Metabisulfite, Stage: `Neutralizing`.
  - [ ] Verify **`GW-9016`**: Recipe `WSH-SOFT-03`, Vintage Peach Softening, Tolon BW-03 (200 kg), 900 pcs, Cationic Softener 3.0 g/L, Stage: `Tumble Drying`.
  - [ ] Verify **`GW-9017`**: Recipe `WSH-RAW-04`, Rinse & De-size Wash, Tolon BW-04 (250 kg), 850 pcs, Stage: `Washing`.

---

## 8. Proper Flow Step 7: Cross-Module Sourcing & Fabric Booking Flow

* **Screen Route:** [`http://localhost:3000/supply`](http://localhost:3000/supply)
* **Frappe Desk Link:** [`http://localhost:8081/app/purchase-order`](http://localhost:8081/app/purchase-order)
* **MariaDB Target:** `tabPurchase Order`

### Checklist:
- [ ] **7.1 Screen Navigation & Form Trigger:**
  - [ ] Navigate to [`http://localhost:3000/supply`](http://localhost:3000/supply).
  - [ ] Locate the **`+ New Fabric Booking`** button and trigger modal.
- [ ] **7.2 Form Input Verification:**
  - [ ] Style / Order: `PO-88214 · MN-PL412`
  - [ ] Mill / Supplier: `Esquire Knit (PTL-GAZ)`
  - [ ] Article: `Single Jersey 160 GSM`
  - [ ] Quantity: `16,240 kg`
  - [ ] Need-by Date: `2026-09-30`
- [ ] **7.3 Submission & Company/Warehouse Parity Audit:**
  - [ ] Submit booking form.
  - [ ] Verify whether target warehouse resolves to active MariaDB warehouse (`Stores - T`) and company (`Textile`).
  - [ ] Note toast feedback or any `LinkValidationError`.

---

## 9. Comprehensive Database Cross-Verification Script

Execute the full verification script in terminal to assert DB integrity across all submodules:

```bash
docker exec gtex-mariadb-1 mariadb -u _a0f2b0ce3c03d38f -pvAbsxKBLGpKarvGE -D _a0f2b0ce3c03d38f -e "
SELECT '=== DYE LOTS ===' AS Table_Header;
SELECT name, dye_lot_number, dye_recipe, shade_band, total_batch_kg, dyeing_machine, rft_status, status 
FROM \`tabDye Lot\` 
ORDER BY creation DESC LIMIT 5;

SELECT '=== FABRIC ROLLS ===' AS Table_Header;
SELECT name, roll_no, yarn_lot, shade_band, net_weight_kg, net_length_m, width_inch, gsm, status 
FROM \`tabFabric Roll\` 
ORDER BY creation DESC LIMIT 5;

SELECT '=== DYE RECIPES ===' AS Table_Header;
SELECT name, recipe_code, shade_band, is_active, total_g_per_kg 
FROM \`tabDye Recipe\`;

SELECT '=== YARN LOTS ===' AS Table_Header;
SELECT name, lot_code, yarn_description, count_ne, lot_weight_kg, status 
FROM \`tabYarn Lot\` LIMIT 5;
"
```

---

## 10. Test Audit Sign-Off Checklist

- [ ] All 6 submodule tabs verified in browser.
- [ ] Form 1 (`+ New Dye Batch`) submitted and verified in MariaDB.
- [ ] Form 2 (`Schedule Machine`) submitted and verified in MariaDB.
- [ ] Form 3 (`Open Re-Dye Job`) submitted and verified in MariaDB.
- [ ] Action (`Release DY-4471`) executed and verified in MariaDB.
- [ ] Dynamic recipe calculator tested and confirmed exact math.
- [ ] MariaDB queries executed and logged in test report.
- [ ] Findings, pass/fail status, and scorecard filed into [`todos and reports/sadat_report_28oct.md`](file:///Users/laptopparadise/Documents/Textile/todos%20and%20reports/sadat_report_28oct.md).
