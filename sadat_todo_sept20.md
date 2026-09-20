# Sadat — 20 Sept 2026 Testing Todo List (Client Presentation Modules)
**Focus Runbooks:**  
- `docs/client_presentation/07_Leave.md` — Leave Management & Entitlements  
- `docs/client_presentation/12_Payroll_Month_End.md` — Payroll Month-End Execution  
- `docs/client_presentation/15_Separation_and_Settlement.md` — Separation & Full & Final Settlement  
**System Target:** `http://172.17.15.132:8080/desk/` (or `http://localhost:8080`, Docker stack `primebank.local`)  
**Testing Methodology:** Live Headed Chromium Browser (`headless: false`, `slowMo: 400ms`), Interactive Slow-Mo Clicks, Glowing Element Outlines (`#00cc66` / `#ff0055` / `#f59e0b`), Floating High-Contrast Step Banners, and Multi-Role Maker–Checker Verification.

---

## Testing Environment & Role Credentials Matrix

| Role | Username / Email | Password | Primary Desk Responsibility |
| :--- | :--- | :--- | :--- |
| **System Administrator** | `Administrator` | `Prime@2026` / `admin` | Setup, roles, permissions, Prime HR Settings, bench executions |
| **HR Maker** | `maker@primebank.com.bd` | `Prime@2026` | Drafts leave rules, encashment, additional salary, increments, payroll entry, separations |
| **HR Checker / HR Manager** | `checker@primebank.com.bd` | `Prime@2026` | Approves leave rules, increment policy & increments, checks & submits payroll entry, approves & completes separations |
| **Accounts Manager** | `administrator` (holds Accounts Manager) | `Prime@2026` | Rung 2: `Validate (Finance)` on Payroll Entry; works F&F Settlement payables |
| **Senior Mgmt / Compliance** | `administrator` (holds Compliance Approver) | `Prime@2026` | Rung 3: `Approve (Senior Mgmt)` on Payroll Entry |
| **Line / Branch Manager** | Manager linked account (e.g. `imran.ahmed@primebank.test`) | `Prime@2026` | Approves leave applications on Team Calendar; files Manager Recommendation for exits |
| **Employee (ESS)** | Linked employee user (e.g. `tanvir.candidate@example.com` / `rubina.akter@primebank.test`) | `Prime@2026` | Applies for leave, submits LWP, files compensatory leave, inspects salary slip |

> [!IMPORTANT]
> **Frappe Desk v15 UI Action Rules:**
> 1. In Frappe Desk v15, workflow transitions (`Submit for Review`, `Approve`, `Check (HR)`, `Validate (Finance)`, `Mark Cleared`, `Complete`) are located inside the top-right **`Actions ▼`** dropdown button (`.actions-btn-group button`).
> 2. Every test walkthrough MUST launch visibly on the desktop screen in headed mode (`headless: false`, `slowMo: 400ms`) with real-time UI banners and element outlines.

---

## 📋 Module 07 — Leave Management & Entitlements
*Source Document:* `docs/client_presentation/07_Leave.md`

- [x] **Step 1: Configure Maker–Checker Leave Eligibility Policy**
  - [x] Log in as **HR Maker** (`maker@primebank.com.bd`).
  - [x] Navigate to `/app/leave-eligibility-rule/new` (top-level link under Leaves).
  - [x] Set **Rule Name**: `Prime Bank Officer Entitlement 2026`.
  - [x] Set **Leave Type**: `Casual Leave`, **Effective From**: `2026-01-01`.
  - [x] Set **Criteria**: `Grade`, **Criteria Value**: `Grade F`, **Action**: `Deny`.
  - [x] Save; submit via **`Actions ▼` ➔ `Submit for Review`** (Draft ➔ Submitted).
  - [x] Log in as **HR Checker** (`checker@primebank.com.bd`).
  - [x] Open rule; click **`Actions ▼` ➔ `Approve`** (Status: `Approved`).
  - [x] Verify that *Deny* rules actively guard applications for matching employee attributes.

- [x] **Step 2: Live Balance Verification & Leave Application (ESS)**
  - [x] Log in as **Employee** (`Tanvir Ahmed` / `rubina.akter@primebank.test`).
  - [x] Navigate to `/app/leave-application/new`.
  - [x] Select Employee and pick **Leave Type** (`Casual Leave`).
  - [x] Verify dynamic read-only display of **Leave Balance Before Application** `[UR-09]` (employee never applies blind).
  - [x] Set **From Date / To Date** (e.g. `2026-09-10` to `2026-09-11`); verify **Total Leave Days** auto-calculates.
  - [x] Add Reason: `Sick — fever and flu; certificate held at branch`.
  - [x] Test hard gates: verify refusal if clashing with Leave Block List dates or if denied by active Eligibility Rule.
  - [x] Click **Save** (parks in status `Open`; ready for approver).

- [x] **Step 3: Multi-Level Manager Approval & Team Calendar Sync**
  - [x] Log in as **Line / Branch Manager** (`imran.ahmed@primebank.test`).
  - [x] Navigate to `/app/leave-application` (filtered `Open`).
  - [x] Inspect applicant details, balance, and reason; click **`Approve`** (or `Reject` with comment).
  - [x] Verify that approval automatically submits the record and deducts balance from employee ledger.
  - [x] Open **Team Leave Calendar** (`/app/team-leave-calendar` under Tenure ➔ Self Service).
  - [x] Confirm employee's approved leave dates immediately render on the team calendar.
  - [x] Verify daily escalation rule: open applications older than `leave_approval_escalation_days` escalate to HR Checker.

- [x] **Step 4: Explicit Leave Without Pay (LWP) with Consent**
  - [x] Log in as **Employee**.
  - [x] Open `/app/leave-application/new` with **Leave Type**: `Leave Without Pay`.
  - [x] Set dates (e.g. `2026-10-05` to `2026-10-09`) and explicit reason.
  - [x] Save and submit application.
  - [x] Verify rule BRL-21: LWP is an explicit, agreed separate application — never a silent conversion of a valid leave.
  - [x] Confirm approved LWP days flow into month-end payroll to deduct payslip pro-rata.

- [x] **Step 5: Compensatory Leave for Worked Holiday**
  - [x] Navigate to `/app/compensatory-leave-request/new` as **Employee**.
  - [x] Select Employee (`Tanvir Ahmed`), Leave Type (`Compensatory Off`).
  - [x] Under *Worked On Holiday*, set **Work From Date** and **Work End Date** (`2026-09-05`).
  - [x] Enter Reason: `Branch open for emergency remittance clearance`.
  - [x] Save; have Line Manager approve.
  - [x] Verify that approval automatically credits 1 earned day to the employee's Leave Allocation balance.

- [x] **Step 6: Annual Leave Planning & Leave Encashment**
  - [x] Navigate to `/app/annual-leave-plan/new` as **Employee**.
  - [x] Set Plan Year `2026`, Leave Type `Earned Leave`, Planned Days `20`; Save.
  - [x] Verify read-only **Taken Days** increments as matching leaves approve (*Planned ➔ Partially Taken ➔ Fully Taken*).
  - [x] Log in as **HR Maker**; navigate to `/app/leave-encashment/new`.
  - [x] Select Leave Period, Employee, Leave Type (`Earned Leave`), and Encashment Days (`10`).
  - [x] Verify auto-computed **Leave Balance**, **Actual Encashable Days**, and **Encashment Amount**.
  - [x] Save and Submit; verify encashment flows to payroll via linked Additional Salary.

- [x] **Step 7: Leave Analytics & Regulatory Audits**
  - [x] Navigate to `/desk/query-report/Leave%20Trend%20Analysis` as HR Checker.
  - [x] Inspect **Absenteeism Analysis** and **Leave Type Usage Analysis**.
  - [x] Run **Leave Balance Summary** report to confirm balances reconcile to the ledger.

---

## 📋 Module 12 — Payroll Month-End Execution
*Source Document:* `docs/client_presentation/12_Payroll_Month_End.md`

- [x] **Step 1: Setup Components, Structures & Salary Assignments**
  - [x] Log in as **HR Maker** (`maker@primebank.com.bd`).
  - [x] Inspect **Salary Components** (`/app/salary-component`):
    - Earnings: `Basic` (BS = base), `House Rent Allowance` (HRA = base * 40 / 100), `Festival Bonus` (FB).
    - Deductions: `Provident Fund` (PF = base * 10 / 100, Fund Classification = `Provident Fund`), `Staff Welfare Fund` (fixed 500 BDT, Fund Classification = `Welfare Fund`), `Income Tax`.
  - [x] Inspect **Salary Structure** `PB Bank Staff Scale 2026` (`/app/salary-structure`).
  - [x] Inspect submitted **Salary Structure Assignment** (`/app/salary-structure-assignment`):
    - e.g. `HR-SSA-26-09-00020` for `Sumaiya Islam` (Base: 38,000 BDT, effective from `2026-01-01`).
  - [x] Confirm hard gate: no salary slip or additional salary computes without an active submitted assignment.

- [x] **Step 2: Ad-Hoc Earnings & Deductions via Additional Salary**
  - [x] Navigate to `/app/additional-salary/new` as **HR Maker**.
  - [x] Select Employee (`Imran Ahmed` / `Sumaiya Islam`), Company (`Prime Bank PLC`), Payroll Date (`2026-09-30`).
  - [x] Pick Component: `Staff Loan Instalment` (deduction) or `Festival Bonus` (earning).
  - [x] Pick Classification: `Loan Adjustment` or `Achievement (JAIBB/DAIBB)`.
  - [x] Enter Detail: `Motorcycle advance instalment 3 of 12` / `Festival Allowance`.
  - [x] Enter Amount (e.g. `4500` BDT) and Submit.
  - [x] Verify queued row carries over automatically to the September payroll run.

- [x] **Step 3: Mid-Month Increments & Arrears Maker–Checker**
  - [x] Navigate to `/app/increment-policy/new` as **HR Maker** (seeded: `PB Increment Policy 2026`).
  - [x] Verify policy fields: Default Annual Increase % (`8%`), **Enforce Grade Increase Limits** checked, Grade-to-Grade min/max rules.
  - [x] Open `/app/increment/new` as **HR Maker**; select Employee (`Sumaiya Islam`).
  - [x] Verify **Current Base** (38,000 BDT) stamps automatically on save.
  - [x] Test raise calculation:
    - In-band: Proposed Base `41040` (8% raise) ➔ **Within Policy Limits** = `Yes`.
    - Out-of-band: Proposed Base `41800` (10% raise) ➔ **Within Policy Limits** = `No (Exception)` requiring **Exception Justification**.
  - [x] HR Maker clicks **`Actions ▼` ➔ `Submit for Review`**.
  - [x] HR Checker opens increment and clicks **`Actions ▼` ➔ `Approve`**.
  - [x] Verify approval automatically generates a **new Salary Structure Assignment** effective-dated from the increment and logs arrears.

- [x] **Step 4: Execute Month-End Payroll Entry & 4-Rung Approval Chain**
  - [x] Navigate to `/app/payroll-entry/new` as **HR Maker**.
  - [x] Set Company: `Prime Bank PLC`, Posting Date: `2026-09-30`, Currency: `BDT`.
  - [x] Set Frequency: `Monthly`, Start Date: `2026-09-01`, End Date: `2026-09-30`.
  - [x] Set Cost Center: `Main - PBP`, Payable Account: `2120 - Payroll Payable - PBP`.
  - [x] Check **Validate Attendance** (errors out on unfinalized attendance).
  - [x] Click **Save**, then click **Get Employees** (populates 24 active bank employees).
  - [x] Click **Create Salary Slips** (generates individual draft slips).
  - [x] Walk the 4-rung maker-checker approval chain:
    - [x] **Rung 1 (HR Check):** Log in as **HR Checker** (`checker@primebank.com.bd`); click **`Actions ▼` ➔ `Check (HR)`** (Draft ➔ `Checked by HR`).
    - [x] **Rung 2 (Finance Validate):** Log in as **Accounts Manager** (`administrator`); click **`Actions ▼` ➔ `Validate (Finance)`** (Checked by HR ➔ `Validated by Finance`).
    - [x] **Rung 3 (Senior Mgmt Approve):** Log in as **Compliance Approver** (`administrator`); click **`Actions ▼` ➔ `Approve (Senior Mgmt)`** (Validated by Finance ➔ `Approved by Senior Management`).
    - [x] **Rung 4 (Final Submit):** Log in as **HR Manager** (`checker@primebank.com.bd`); click **`Actions ▼` ➔ `Submit`** (Approved ➔ `Submitted`).
  - [x] Verify hard gate: submit throws if any salary slip is still in Draft state.
  - [x] Confirm all salary slips transition to immutable `Submitted` state.

- [x] **Step 5: CBS Core Banking Advice File, Tax Certificates & Funds**
  - [x] On the submitted Payroll Entry, verify the green **CBS Disbursement File** button appears.
  - [x] Click **CBS Disbursement File** dialog; verify batch rows (Employee, Bank Name, Account Number, Net Pay).
  - [x] Click **Download CSV** (`cbs_disbursement_<entry>.csv`) driven by `prime_bank_hrms.api.get_cbs_disbursement_file`.
  - [x] Navigate to `/app/tax-certificate/new` as HR Checker; select employee and fiscal year `2025-2026`.
  - [x] Verify auto-aggregation of Total Gross Earning, Total Tax Deducted, e-TIN, and Tax Zone.
  - [x] Inspect **Provident Fund Account** and **Staff Fund Ledger**; confirm PF EE/ER splits and welfare postings.
  - [x] Open **PF and Fund Statement** (`/desk/query-report/PF%20and%20Fund%20Statement`) to reconcile fund totals to slips.

- [x] **Step 6: Employee Reads Audited Salary Slip (ESS)**
  - [x] Log in as **Employee** (`Sumaiya Islam` / `imran.ahmed@primebank.test`).
  - [x] Open submitted **Salary Slip** (`/app/salary-slip`).
  - [x] Verify itemized breakdown: Basic, HRA, Festival Bonus, PF deduction (10%), Welfare deduction (500 BDT), Tax withholding, and Net Pay.
  - [x] Confirm slip is strictly read-only and immutable.

---

## 📋 Module 15 — Separation & Full & Final Settlement
*Source Document:* `docs/client_presentation/15_Separation_and_Settlement.md`

- [x] **Step 1: Separation Configuration & Clearance Checklist Template**
  - [x] Log in as **HR Maker** (`maker@primebank.com.bd`).
  - [x] Inspect **Separation Type** (`/app/separation-type`):
    - `Resignation`: Requires Notice = `Yes`, Default Notice = `90 days`, Allow Withdrawal = `Yes`, Blocks Rehire = `No`, Exempt from Clearance = `No`.
    - Special categories: `Death in Service` (clearance-exempt), `Termination` / `Dismissal` (blocks rehire).
  - [x] Inspect **Clearance Checklist Template** (`/app/clearance-checklist-template`):
    - Open `Standard Exit Clearance` for `Prime Bank PLC`.
    - Verify mandatory items across all 6 departments: **HR**, **IT**, **Administration**, **Finance**, **Security**, and **Business Unit**.

- [x] **Step 2: Record Separation & Automatic Notice Computation**
  - [x] Navigate to `/app/employee-separation/new` as **HR Maker**.
  - [x] Select Employee (`Tabassum Rahman`, `HR-EMP-00006`) and Separation Type (`Resignation`).
  - [x] Set **Separation Begins On** (relieving date).
  - [x] Click **Save**; verify automatic calculation of notice block:
    - **Required Notice (days)**: `90`
    - **Notice Start Date**: resignation date / today
    - **Notice Completion Date**: start date + 90 days
    - **Shortfall (days)**: completion date − relieving date
    - **Buy-out Amount**: shortfall × daily basic (base ÷ 30).
  - [x] Confirm that a waiver ticked on a type that forbids waivers refuses at save.

- [x] **Step 3: Manager Recommendation & Prerequisite Workflow Chain**
  - [x] Log in as **Line Manager**; navigate to `/app/manager-recommendation/new`.
  - [x] Fill Recommending Manager, Employee (`Tabassum Rahman`), Recommendation Type (`Separation`), Justification.
  - [x] Manager clicks **`Recommend`**.
  - [x] Log in as **HR Checker** (`checker@primebank.com.bd`); open recommendation and click **`Actions ▼` ➔ `Accept`**.
  - [x] Log in as **HR Maker**; open the Employee Separation record and click **`Actions ▼` ➔ `Recommend`**.
  - [x] Test hard gate: verify that clicking Recommend without an accepted Manager Recommendation is strictly refused by the server.
  - [x] Log in as **HR Checker**; click **`Actions ▼` ➔ `Approve`**.
  - [x] Confirm that **Approve** automatically creates:
    - Linked **Exit Clearance** populated with items from the template.
    - Linked draft **Full and Final Statement** with auto-suggested receivables.

- [x] **Step 4: Multi-Department Exit Clearance Execution**
  - [x] Open the linked **Exit Clearance** record (`/app/exit-clearance/<linked>`).
  - [x] Simulate clearance execution across all 6 departments:
    - [x] **HR**: Leave balance audit & encashment review ➔ check **Cleared** with date & user.
    - [x] **IT**: Laptop return, ID card, email & active directory deactivation ➔ check **Cleared**.
    - [x] **Administration**: Vehicle & physical assets return ➔ check **Cleared**.
    - [x] **Finance**: Outstanding staff loans & advances review ➔ check **Cleared**.
    - [x] **Security**: Access card returned, vault access revoked ➔ check **Cleared**.
    - [x] **Business Unit**: Client portfolio & document handover ➔ check **Cleared**.
  - [x] Test asset recovery: for branch laptop row, set `Is Asset` = ✓, `Condition` = `Damaged`, `Recovery Amount` = `15000` BDT.
  - [x] Verify **Asset Recovery Total** computes and status automatically flips to `Completed` when all mandatory items are done.

- [x] **Step 5: Mark Cleared & Complete Separation Submit**
  - [x] Return to the **Employee Separation** record as **HR Checker**.
  - [x] Click **`Actions ▼` ➔ `Mark Cleared`** (server re-validates that all mandatory items are cleared).
  - [x] Click **`Actions ▼` ➔ `Complete`** to execute final submission.
  - [x] Test hard gates:
    - Attempting Complete while clearance items are pending throws error.
    - Attempting Complete with unapproved notice shortfall throws error.
  - [x] Verify automated single-transaction cascade on submit:
    - [x] Employee status flipped to **`Left`** (mapped detailed status: `Resigned`).
    - [x] All active **Employee Position Assignments** relieved (position returned to vacancy register).
    - [x] Linked Frappe **User desk account disabled** (IAM de-provisioning).
    - [x] **Alumni record auto-created** with consent tracking.

- [x] **Step 6: Full and Final (F&F) Statement & Exit Interview**
  - [x] Open the linked **Full and Final Statement**.
  - [x] Verify auto-surfaced **Receivables**: notice buyout amount, asset recovery total (15,000 BDT), and outstanding staff loan balance.
  - [x] Log in as **Finance Officer**; add **Payables**:
    - Gratuity (e.g. `180,000` BDT)
    - Leave Encashment (e.g. `35,000` BDT)
    - Prorated Festival Bonus (e.g. `12,500` BDT)
  - [x] Set each row status to `Settled`, verify net payable/receivable, and click **Submit**.
  - [x] Log in as **HR Maker**; navigate to `/app/exit-interview/new`.
  - [x] Record date, interview panel, question template responses, and Primary Reason (`Better Opportunity`).
  - [x] Save and submit exit interview; confirm data feeds **Exit Interview Trends** and **Attrition Analysis**.

- [x] **Step 7: Issue Separation Letters with Public Verification Token**
  - [x] Log in as **HR Checker**; navigate to `/app/separation-letter/new`.
  - [x] Create letter: Letter Type `Resignation Acceptance Letter`, Employee `Tabassum Rahman`, linked Separation.
  - [x] Save; verify system generates unique alphanumeric **Verification Token**.
  - [x] Test additional letter types: `Relieving Letter`, `Experience Certificate`, `No Dues Certificate`.
  - [x] Open public verification URL: `http://172.17.15.132:8080/verify_letter?token=<token>`.
  - [x] Verify that external auditors/employers can validate letter authenticity without login.
