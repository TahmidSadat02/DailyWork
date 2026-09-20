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

- [ ] **Step 1: Configure Maker–Checker Leave Eligibility Policy**
  - [ ] Log in as **HR Maker** (`maker@primebank.com.bd`).
  - [ ] Navigate to `/app/leave-eligibility-rule/new` (top-level link under Leaves).
  - [ ] Set **Rule Name**: `Prime Bank Officer Entitlement 2026`.
  - [ ] Set **Leave Type**: `Casual Leave`, **Effective From**: `2026-01-01`.
  - [ ] Set **Criteria**: `Grade`, **Criteria Value**: `Grade F`, **Action**: `Deny`.
  - [ ] Save; submit via **`Actions ▼` ➔ `Submit for Review`** (Draft ➔ Submitted).
  - [ ] Log in as **HR Checker** (`checker@primebank.com.bd`).
  - [ ] Open rule; click **`Actions ▼` ➔ `Approve`** (Status: `Approved`).
  - [ ] Verify that *Deny* rules actively guard applications for matching employee attributes.

- [ ] **Step 2: Live Balance Verification & Leave Application (ESS)**
  - [ ] Log in as **Employee** (`Tanvir Ahmed` / `rubina.akter@primebank.test`).
  - [ ] Navigate to `/app/leave-application/new`.
  - [ ] Select Employee and pick **Leave Type** (`Casual Leave`).
  - [ ] Verify dynamic read-only display of **Leave Balance Before Application** `[UR-09]` (employee never applies blind).
  - [ ] Set **From Date / To Date** (e.g. `2026-09-10` to `2026-09-11`); verify **Total Leave Days** auto-calculates.
  - [ ] Add Reason: `Sick — fever and flu; certificate held at branch`.
  - [ ] Test hard gates: verify refusal if clashing with Leave Block List dates or if denied by active Eligibility Rule.
  - [ ] Click **Save** (parks in status `Open`; ready for approver).

- [ ] **Step 3: Multi-Level Manager Approval & Team Calendar Sync**
  - [ ] Log in as **Line / Branch Manager** (`imran.ahmed@primebank.test`).
  - [ ] Navigate to `/app/leave-application` (filtered `Open`).
  - [ ] Inspect applicant details, balance, and reason; click **`Approve`** (or `Reject` with comment).
  - [ ] Verify that approval automatically submits the record and deducts balance from employee ledger.
  - [ ] Open **Team Leave Calendar** (`/app/team-leave-calendar` under Tenure ➔ Self Service).
  - [ ] Confirm employee's approved leave dates immediately render on the team calendar.
  - [ ] Verify daily escalation rule: open applications older than `leave_approval_escalation_days` escalate to HR Checker.

- [ ] **Step 4: Explicit Leave Without Pay (LWP) with Consent**
  - [ ] Log in as **Employee**.
  - [ ] Open `/app/leave-application/new` with **Leave Type**: `Leave Without Pay`.
  - [ ] Set dates (e.g. `2026-10-05` to `2026-10-09`) and explicit reason.
  - [ ] Save and submit application.
  - [ ] Verify rule BRL-21: LWP is an explicit, agreed separate application — never a silent conversion of a valid leave.
  - [ ] Confirm approved LWP days flow into month-end payroll to deduct payslip pro-rata.

- [ ] **Step 5: Compensatory Leave for Worked Holiday**
  - [ ] Navigate to `/app/compensatory-leave-request/new` as **Employee**.
  - [ ] Select Employee (`Tanvir Ahmed`), Leave Type (`Compensatory Off`).
  - [ ] Under *Worked On Holiday*, set **Work From Date** and **Work End Date** (`2026-09-05`).
  - [ ] Enter Reason: `Branch open for emergency remittance clearance`.
  - [ ] Save; have Line Manager approve.
  - [ ] Verify that approval automatically credits 1 earned day to the employee's Leave Allocation balance.

- [ ] **Step 6: Annual Leave Planning & Leave Encashment**
  - [ ] Navigate to `/app/annual-leave-plan/new` as **Employee**.
  - [ ] Set Plan Year `2026`, Leave Type `Earned Leave`, Planned Days `20`; Save.
  - [ ] Verify read-only **Taken Days** increments as matching leaves approve (*Planned ➔ Partially Taken ➔ Fully Taken*).
  - [ ] Log in as **HR Maker**; navigate to `/app/leave-encashment/new`.
  - [ ] Select Leave Period, Employee, Leave Type (`Earned Leave`), and Encashment Days (`10`).
  - [ ] Verify auto-computed **Leave Balance**, **Actual Encashable Days**, and **Encashment Amount**.
  - [ ] Save and Submit; verify encashment flows to payroll via linked Additional Salary.

- [ ] **Step 7: Leave Analytics & Regulatory Audits**
  - [ ] Navigate to `/desk/query-report/Leave%20Trend%20Analysis` as HR Checker.
  - [ ] Inspect **Absenteeism Analysis** and **Leave Type Usage Analysis**.
  - [ ] Run **Leave Balance Summary** report to confirm balances reconcile to the ledger.

---

## 📋 Module 12 — Payroll Month-End Execution
*Source Document:* `docs/client_presentation/12_Payroll_Month_End.md`

- [ ] **Step 1: Setup Components, Structures & Salary Assignments**
  - [ ] Log in as **HR Maker** (`maker@primebank.com.bd`).
  - [ ] Inspect **Salary Components** (`/app/salary-component`):
    - Earnings: `Basic` (BS = base), `House Rent Allowance` (HRA = base * 40 / 100), `Festival Bonus` (FB).
    - Deductions: `Provident Fund` (PF = base * 10 / 100, Fund Classification = `Provident Fund`), `Staff Welfare Fund` (fixed 500 BDT, Fund Classification = `Welfare Fund`), `Income Tax`.
  - [ ] Inspect **Salary Structure** `PB Bank Staff Scale 2026` (`/app/salary-structure`).
  - [ ] Inspect submitted **Salary Structure Assignment** (`/app/salary-structure-assignment`):
    - e.g. `HR-SSA-26-09-00020` for `Sumaiya Islam` (Base: 38,000 BDT, effective from `2026-01-01`).
  - [ ] Confirm hard gate: no salary slip or additional salary computes without an active submitted assignment.

- [ ] **Step 2: Ad-Hoc Earnings & Deductions via Additional Salary**
  - [ ] Navigate to `/app/additional-salary/new` as **HR Maker**.
  - [ ] Select Employee (`Imran Ahmed` / `Sumaiya Islam`), Company (`Prime Bank PLC`), Payroll Date (`2026-09-30`).
  - [ ] Pick Component: `Staff Loan Instalment` (deduction) or `Festival Bonus` (earning).
  - [ ] Pick Classification: `Loan Adjustment` or `Achievement (JAIBB/DAIBB)`.
  - [ ] Enter Detail: `Motorcycle advance instalment 3 of 12` / `Festival Allowance`.
  - [ ] Enter Amount (e.g. `4500` BDT) and Submit.
  - [ ] Verify queued row carries over automatically to the September payroll run.

- [ ] **Step 3: Mid-Month Increments & Arrears Maker–Checker**
  - [ ] Navigate to `/app/increment-policy/new` as **HR Maker** (seeded: `PB Increment Policy 2026`).
  - [ ] Verify policy fields: Default Annual Increase % (`8%`), **Enforce Grade Increase Limits** checked, Grade-to-Grade min/max rules.
  - [ ] Open `/app/increment/new` as **HR Maker**; select Employee (`Sumaiya Islam`).
  - [ ] Verify **Current Base** (38,000 BDT) stamps automatically on save.
  - [ ] Test raise calculation:
    - In-band: Proposed Base `41040` (8% raise) ➔ **Within Policy Limits** = `Yes`.
    - Out-of-band: Proposed Base `41800` (10% raise) ➔ **Within Policy Limits** = `No (Exception)` requiring **Exception Justification**.
  - [ ] HR Maker clicks **`Actions ▼` ➔ `Submit for Review`**.
  - [ ] HR Checker opens increment and clicks **`Actions ▼` ➔ `Approve`**.
  - [ ] Verify approval automatically generates a **new Salary Structure Assignment** effective-dated from the increment and logs arrears.

- [ ] **Step 4: Execute Month-End Payroll Entry & 4-Rung Approval Chain**
  - [ ] Navigate to `/app/payroll-entry/new` as **HR Maker**.
  - [ ] Set Company: `Prime Bank PLC`, Posting Date: `2026-09-30`, Currency: `BDT`.
  - [ ] Set Frequency: `Monthly`, Start Date: `2026-09-01`, End Date: `2026-09-30`.
  - [ ] Set Cost Center: `Main - PBP`, Payable Account: `2120 - Payroll Payable - PBP`.
  - [ ] Check **Validate Attendance** (errors out on unfinalized attendance).
  - [ ] Click **Save**, then click **Get Employees** (populates 24 active bank employees).
  - [ ] Click **Create Salary Slips** (generates individual draft slips).
  - [ ] Walk the 4-rung maker-checker approval chain:
    - [ ] **Rung 1 (HR Check):** Log in as **HR Checker** (`checker@primebank.com.bd`); click **`Actions ▼` ➔ `Check (HR)`** (Draft ➔ `Checked by HR`).
    - [ ] **Rung 2 (Finance Validate):** Log in as **Accounts Manager** (`administrator`); click **`Actions ▼` ➔ `Validate (Finance)`** (Checked by HR ➔ `Validated by Finance`).
    - [ ] **Rung 3 (Senior Mgmt Approve):** Log in as **Compliance Approver** (`administrator`); click **`Actions ▼` ➔ `Approve (Senior Mgmt)`** (Validated by Finance ➔ `Approved by Senior Management`).
    - [ ] **Rung 4 (Final Submit):** Log in as **HR Manager** (`checker@primebank.com.bd`); click **`Actions ▼` ➔ `Submit`** (Approved ➔ `Submitted`).
  - [ ] Verify hard gate: submit throws if any salary slip is still in Draft state.
  - [ ] Confirm all salary slips transition to immutable `Submitted` state.

- [ ] **Step 5: CBS Core Banking Advice File, Tax Certificates & Funds**
  - [ ] On the submitted Payroll Entry, verify the green **CBS Disbursement File** button appears.
  - [ ] Click **CBS Disbursement File** dialog; verify batch rows (Employee, Bank Name, Account Number, Net Pay).
  - [ ] Click **Download CSV** (`cbs_disbursement_<entry>.csv`) driven by `prime_bank_hrms.api.get_cbs_disbursement_file`.
  - [ ] Navigate to `/app/tax-certificate/new` as HR Checker; select employee and fiscal year `2025-2026`.
  - [ ] Verify auto-aggregation of Total Gross Earning, Total Tax Deducted, e-TIN, and Tax Zone.
  - [ ] Inspect **Provident Fund Account** and **Staff Fund Ledger**; confirm PF EE/ER splits and welfare postings.
  - [ ] Open **PF and Fund Statement** (`/desk/query-report/PF%20and%20Fund%20Statement`) to reconcile fund totals to slips.

- [ ] **Step 6: Employee Reads Audited Salary Slip (ESS)**
  - [ ] Log in as **Employee** (`Sumaiya Islam` / `imran.ahmed@primebank.test`).
  - [ ] Open submitted **Salary Slip** (`/app/salary-slip`).
  - [ ] Verify itemized breakdown: Basic, HRA, Festival Bonus, PF deduction (10%), Welfare deduction (500 BDT), Tax withholding, and Net Pay.
  - [ ] Confirm slip is strictly read-only and immutable.

---

## 📋 Module 15 — Separation & Full & Final Settlement
*Source Document:* `docs/client_presentation/15_Separation_and_Settlement.md`

- [ ] **Step 1: Separation Configuration & Clearance Checklist Template**
  - [ ] Log in as **HR Maker** (`maker@primebank.com.bd`).
  - [ ] Inspect **Separation Type** (`/app/separation-type`):
    - `Resignation`: Requires Notice = `Yes`, Default Notice = `90 days`, Allow Withdrawal = `Yes`, Blocks Rehire = `No`, Exempt from Clearance = `No`.
    - Special categories: `Death in Service` (clearance-exempt), `Termination` / `Dismissal` (blocks rehire).
  - [ ] Inspect **Clearance Checklist Template** (`/app/clearance-checklist-template`):
    - Open `Standard Exit Clearance` for `Prime Bank PLC`.
    - Verify mandatory items across all 6 departments: **HR**, **IT**, **Administration**, **Finance**, **Security**, and **Business Unit**.

- [ ] **Step 2: Record Separation & Automatic Notice Computation**
  - [ ] Navigate to `/app/employee-separation/new` as **HR Maker**.
  - [ ] Select Employee (`Tabassum Rahman`, `HR-EMP-00006`) and Separation Type (`Resignation`).
  - [ ] Set **Separation Begins On** (relieving date).
  - [ ] Click **Save**; verify automatic calculation of notice block:
    - **Required Notice (days)**: `90`
    - **Notice Start Date**: resignation date / today
    - **Notice Completion Date**: start date + 90 days
    - **Shortfall (days)**: completion date − relieving date
    - **Buy-out Amount**: shortfall × daily basic (base ÷ 30).
  - [ ] Confirm that a waiver ticked on a type that forbids waivers refuses at save.

- [ ] **Step 3: Manager Recommendation & Prerequisite Workflow Chain**
  - [ ] Log in as **Line Manager**; navigate to `/app/manager-recommendation/new`.
  - [ ] Fill Recommending Manager, Employee (`Tabassum Rahman`), Recommendation Type (`Separation`), Justification.
  - [ ] Manager clicks **`Recommend`**.
  - [ ] Log in as **HR Checker** (`checker@primebank.com.bd`); open recommendation and click **`Actions ▼` ➔ `Accept`**.
  - [ ] Log in as **HR Maker**; open the Employee Separation record and click **`Actions ▼` ➔ `Recommend`**.
  - [ ] Test hard gate: verify that clicking Recommend without an accepted Manager Recommendation is strictly refused by the server.
  - [ ] Log in as **HR Checker**; click **`Actions ▼` ➔ `Approve`**.
  - [ ] Confirm that **Approve** automatically creates:
    - Linked **Exit Clearance** populated with items from the template.
    - Linked draft **Full and Final Statement** with auto-suggested receivables.

- [ ] **Step 4: Multi-Department Exit Clearance Execution**
  - [ ] Open the linked **Exit Clearance** record (`/app/exit-clearance/<linked>`).
  - [ ] Simulate clearance execution across all 6 departments:
    - [ ] **HR**: Leave balance audit & encashment review ➔ check **Cleared** with date & user.
    - [ ] **IT**: Laptop return, ID card, email & active directory deactivation ➔ check **Cleared**.
    - [ ] **Administration**: Vehicle & physical assets return ➔ check **Cleared**.
    - [ ] **Finance**: Outstanding staff loans & advances review ➔ check **Cleared**.
    - [ ] **Security**: Access card returned, vault access revoked ➔ check **Cleared**.
    - [ ] **Business Unit**: Client portfolio & document handover ➔ check **Cleared**.
  - [ ] Test asset recovery: for branch laptop row, set `Is Asset` = ✓, `Condition` = `Damaged`, `Recovery Amount` = `15000` BDT.
  - [ ] Verify **Asset Recovery Total** computes and status automatically flips to `Completed` when all mandatory items are done.

- [ ] **Step 5: Mark Cleared & Complete Separation Submit**
  - [ ] Return to the **Employee Separation** record as **HR Checker**.
  - [ ] Click **`Actions ▼` ➔ `Mark Cleared`** (server re-validates that all mandatory items are cleared).
  - [ ] Click **`Actions ▼` ➔ `Complete`** to execute final submission.
  - [ ] Test hard gates:
    - Attempting Complete while clearance items are pending throws error.
    - Attempting Complete with unapproved notice shortfall throws error.
  - [ ] Verify automated single-transaction cascade on submit:
    - [ ] Employee status flipped to **`Left`** (mapped detailed status: `Resigned`).
    - [ ] All active **Employee Position Assignments** relieved (position returned to vacancy register).
    - [ ] Linked Frappe **User desk account disabled** (IAM de-provisioning).
    - [ ] **Alumni record auto-created** with consent tracking.

- [ ] **Step 6: Full and Final (F&F) Statement & Exit Interview**
  - [ ] Open the linked **Full and Final Statement**.
  - [ ] Verify auto-surfaced **Receivables**: notice buyout amount, asset recovery total (15,000 BDT), and outstanding staff loan balance.
  - [ ] Log in as **Finance Officer**; add **Payables**:
    - Gratuity (e.g. `180,000` BDT)
    - Leave Encashment (e.g. `35,000` BDT)
    - Prorated Festival Bonus (e.g. `12,500` BDT)
  - [ ] Set each row status to `Settled`, verify net payable/receivable, and click **Submit**.
  - [ ] Log in as **HR Maker**; navigate to `/app/exit-interview/new`.
  - [ ] Record date, interview panel, question template responses, and Primary Reason (`Better Opportunity`).
  - [ ] Save and submit exit interview; confirm data feeds **Exit Interview Trends** and **Attrition Analysis**.

- [ ] **Step 7: Issue Separation Letters with Public Verification Token**
  - [ ] Log in as **HR Checker**; navigate to `/app/separation-letter/new`.
  - [ ] Create letter: Letter Type `Resignation Acceptance Letter`, Employee `Tabassum Rahman`, linked Separation.
  - [ ] Save; verify system generates unique alphanumeric **Verification Token**.
  - [ ] Test additional letter types: `Relieving Letter`, `Experience Certificate`, `No Dues Certificate`.
  - [ ] Open public verification URL: `http://172.17.15.132:8080/verify_letter?token=<token>`.
  - [ ] Verify that external auditors/employers can validate letter authenticity without login.
