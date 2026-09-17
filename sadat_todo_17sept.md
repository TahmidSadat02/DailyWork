# Sadat — 17 Sept 2026 Testing Todo List (Client Presentation Modules)
**Folder to Test Today:** `docs/client_presentation/`  
**System Target:** `http://172.17.15.132:8080/desk/` (or `http://localhost:8080`, Docker stack `primebank.local`)  
**Methodology:** Headed Visual Browser Walkthrough (`headless: false`, `slowMo: 400ms`) with Glowing Element Outlines, On-Screen Step Banners, and Maker–Checker Verification.

---

## Testing Environment & Credentials

| Role | Email / Username | Password | Purpose & Notes |
| :--- | :--- | :--- | :--- |
| **System Administrator** | `Administrator` | `Prime@2026` / `admin` | Setup, role permissions, Prime HR Settings, bench jobs |
| **HR Maker** | `maker@primebank.com.bd` | `Prime@2026` | Drafts org masters, positions, requisitions, onboarding, attendance, salary entries *(Company: Prime Bank PLC / UniSoft)* |
| **HR Checker** | `checker@primebank.com.bd` | `Prime@2026` | Approves requests, submits onboarding, confirms probation, executes month-end payroll |
| **Compliance Approver** | `compliance@primebank.com.bd` (or Admin) | `Prime@2026` | Approves critical position requests and management rungs |
| **Employee (ESS)** | Linked employee email (e.g. `tanvir.candidate@example.com` / `rubina.akter@primebank.test`) | `Prime@2026` | Self-service attendance regularization, leave, payslip inspection |
| **Line / Branch Manager** | Manager email (e.g. `imran.ahmed@primebank.test`) | `Prime@2026` | Recommends separation, approves leave/punch regularization |

> [!IMPORTANT]
> **Key Frappe Desk UI Control Rules:**
> 1. In Frappe Desk v15, workflow transitions (`Submit for Review`, `Approve`, `Reject`, `Recommend`) appear inside the top-right **`Actions ▼`** dropdown button (`.actions-btn-group button`).
> 2. Every test walkthrough MUST run in headed browser mode (`headless: false`) with step-by-step UI highlighting and delays so the flow is visible live on screen.

---

## 📋 Module 01 — Position & Headcount Management [COMPLETED ✅]
*Source Document:* `docs/client_presentation/01_Position_and_Headcount.md`

- [x] **Step 1: Org Masters Hierarchy**
  - [x] Log in as **HR Maker** (`maker@primebank.com.bd`).
  - [x] Inspect/Verify **Zone**, **Region**, and **Business Unit** titles (`/app/zone`, `/app/region`, `/app/business-unit`).
  - [x] Inspect **Branch** (`Dhaka Main` with Region `Dhaka Region`, Zone `Dhaka Zone`, Banking Type `Conventional`).
  - [x] Inspect **Department** (`Retail Operations - PBP` linked to Business Unit `Retail Banking` and Branch `Dhaka Main`).
  - [x] Inspect **Designation** (`Customer Service Officer` with Default Grade `Grade F`).
- [x] **Step 2: Create Independent Position Master**
  - [x] Navigate to `/app/position/new` / inspect `POS-2026-00001`.
  - [x] Create/Inspect Position: Title `Customer Service Officer — Dhaka Main`, Designation `Customer Service Officer`, Company `Prime Bank PLC`, Branch `Dhaka Main`.
  - [x] Verify `Job Grade` auto-fetches `Grade F` and status defaults to `Vacant` with `vacant_since` stamped.
  - [x] Verify Approved Incumbents capacity, Reports To (`POS-2026-00020`), and vacancy tracking.
- [x] **Step 3: Maker–Checker Structural Change (`Position Request`)**
  - [x] Navigate to `/app/position-request/new` as **HR Maker**.
  - [x] Create Position Request: Type `New Position`, Title `Cards Operations Officer — Dhaka (#989)`, Approved Incumbents `2`, Reason recorded.
  - [x] Submit via **`Actions ▼` ➔ `Submit for Review`** (Draft ➔ Submitted: `PR-2026-00011`).
  - [x] Log in as **HR Checker** (`checker@primebank.com.bd`).
  - [x] Open request, verify timeline, and click **`Actions ▼` ➔ `Approve`** (materializes new position `POS-2026-00053` with Position Log entry).
  - [x] Verify direct status changes on Position (Freeze / Abolish) are restricted to Checker or require requests.
- [x] **Step 4: Fill and Relieve (`Employee Position Assignment`)**
  - [x] Navigate to `/app/employee-position-assignment`.
  - [x] Inspect active assignment binding employee to position.
  - [x] Verify status flips to `Filled` and capacity reflects incumbent.
  - [x] Inspect the **`Relieve`** button action returning post to vacancy register.
- [x] **Step 5: Vacancy Aging & Establishment Reports**
  - [x] Run **Vacancy Report** (`/desk/query-report/Vacancy%20Report`); verify days vacant and aging buckets (0–30, 31–60, 61–90, 90+).
  - [x] Run **Filled vs Vacant Positions** (`/desk/query-report/Filled%20vs%20Vacant%20Positions`) & verify Establishment vs Actual counts.
- [x] **Step 6: Regulatory Bangladesh Bank (BB) Manpower Return**
  - [x] Log in as **HR Checker**; navigate to **Regulatory Manpower Report** (`/desk/query-report/Regulatory%20Manpower%20Report`).
  - [x] Verify report groups by Business Unit / Branch / Designation, splits Conventional vs Islamic, and outputs exact totals straight from live data without spreadsheets (34 Approved, 1 Filled, 33 Vacant).

---

## 📋 Module 02 — Identity, Access, and Audit Trail
*Source Document:* `docs/client_presentation/02_Identity_Access_and_Audit.md`

- [ ] **Step 1: Inspect Complete Audit Trail & Log Masters**
  - [ ] Open any governed record (e.g. Employee master); open the **Versions** list in the form sidebar.
  - [ ] Inspect **Position Log** (`/app/position-log`) showing who, what, when, before ➔ after.
  - [ ] Inspect document timelines; verify logs are read-only even for System Administrators.
- [ ] **Step 2: Two-Person Rule (Maker–Checker Bidirectional Enforcement)**
  - [ ] As **HR Maker**, create an **Employee Change Request** (`/app/employee-change-request/new`).
  - [ ] Submit for review via **`Actions ▼` ➔ `Submit for Review`**.
  - [ ] Observe that the `Approve` button is strictly **absent** from Maker's Actions menu (anti self-approval).
  - [ ] As **HR Checker**, open the request, inspect field-level before/after diffs, and click **`Actions ▼` ➔ `Approve`**.
  - [ ] Verify updates commit to Employee master and write to **Employee Change Log**.
- [ ] **Step 3: Automatic User Provisioning on Hire**
  - [ ] Verify *Enable User Provisioning* setting in **Prime HR Settings** (`/app/prime-hr-settings`).
  - [ ] Verify that upon creating/confirming a new employee, a linked Frappe `User` with standard ESS role is auto-generated without an IT ticket.
- [ ] **Step 4: Automatic User De-provisioning on Exit**
  - [ ] Verify that completing an Employee Separation disables the linked `User` record in the exact same transaction.
- [ ] **Step 5: Role-Based & Field-Level Security**
  - [ ] Inspect sensitive fields on Employee master (National ID / NID, e-TIN, Passport); verify they are hidden/restricted for unauthorized roles.
  - [ ] Verify portal user self-scoping (users cannot view others' records).
- [ ] **Step 6: Approval Delegation with Auto-Revert**
  - [ ] Navigate to `/app/approval-delegation/new`.
  - [ ] Set From (Manager), To (Delegate), start date and future end date.
  - [ ] Verify delegation auto-expires and reverts approval rights after the end date passes.

---

## 📋 Module 03 — Recruitment & Candidate Journey
*Source Document:* `docs/client_presentation/03_Recruitment.md`

- [ ] **Step 1: Vacancy Verification Gate**
  - [ ] Verify that a `Vacant` position exists before raising manpower requisition (`/app/position`).
- [ ] **Step 2: Raise Job Requisition**
  - [ ] Navigate to `/app/job-requisition/new` as **HR Maker**.
  - [ ] Fill Designation (`Customer Service Officer`), No. of Positions (`1`), Expected Compensation (`35,000`), link the vacant Position, Company (`Prime Bank PLC`).
  - [ ] Save; verify Estimated Budget auto-calculates and validates against Staffing Plan.
- [ ] **Step 3: Multi-Rung Requisition Approval Chain**
  - [ ] HR Maker clicks **`Actions ▼` ➔ `Submit for Review`** (Pending ➔ Submitted).
  - [ ] HR Checker clicks **`Actions ▼` ➔ `Recommend (Division Head)`**.
  - [ ] HR Checker clicks **`Actions ▼` ➔ `Approve (HR)`**.
  - [ ] Management / Compliance Approver clicks **`Actions ▼` ➔ `Approve (Management)`**.
  - [ ] HR Checker clicks **`Actions ▼` ➔ `Open Requisition`** (Status flips to `Open & Approved`).
- [ ] **Step 4: Auto-Created Job Opening & Pre-Screening Questions**
  - [ ] Open auto-created **Job Opening** (`/app/job-opening`).
  - [ ] Add Pre-Screening questions (e.g. *Minimum education?*, *CSR experience?* with Knock-Out flag).
- [ ] **Step 5: Candidate Capture & Pre-Screening Evaluation**
  - [ ] Create **Job Applicant** (`/app/job-applicant/new`) for candidate `Tanvir Ahmed` with email and resume.
  - [ ] Save; verify pre-screening evaluates and `Status Tracking Token` is generated.
- [ ] **Step 6: Public Candidate Status Tracking**
  - [ ] Open public applicant portal URL (`/track_application?token=<token>`).
  - [ ] Verify stage progression is visible to candidate without login.
- [ ] **Step 7: Shortlisting & Panel Interview Scoring**
  - [ ] Filter applicants by `Screening Passed = Yes`; update status to `Shortlisted`.
  - [ ] Schedule **Interview** (`/app/interview/new`) with panel interviewers.
  - [ ] Submit feedback and verify average panel score rollup.
- [ ] **Step 8: Issue Job Offer**
  - [ ] Navigate to `/app/job-offer/new` (or click `Create Job Offer` from Applicant).
  - [ ] Configure compensation terms and save/submit in status `Awaiting Response`.
  - [ ] Simulate candidate acceptance (status: `Accepted`).

---

## 📋 Module 04 — Onboarding & Probation Confirmation
*Source Document:* `docs/client_presentation/04_Onboarding.md`

- [ ] **Step 1: Onboarding Template Configuration**
  - [ ] Inspect/create **Employee Onboarding Template** (`/app/employee-onboarding-template`) with activities: `IT Account Creation`, `ID Card Issuance`, `Desk Allocation`, `Induction Session`.
  - [ ] Verify activity flag `Required for Employee Creation`.
- [ ] **Step 2: Initiate Employee Onboarding Record**
  - [ ] Navigate to `/app/employee-onboarding/new` as **HR Maker**.
  - [ ] Select accepted Job Applicant (`Tanvir Ahmed`) and accepted Job Offer (`HR-OFF-2026-00002`).
  - [ ] Select template; Save, and have **HR Checker** click **Submit** (spawns Onboarding Project & Tasks).
- [ ] **Step 3: Employee Record Creation & Position Assignment**
  - [ ] On submitted onboarding, click **`Create ➔ Employee`** (hard-gated on required pre-creation tasks).
  - [ ] Save the generated Employee master.
  - [ ] File **Employee Position Assignment** linking the new employee to their vacant Position.
- [ ] **Step 4: Checklist Execution & Document Collection**
  - [ ] Complete onboarding tasks; track Required Documents (National ID, Educational Certificates).
  - [ ] Record Code of Conduct Booklet delivery and acknowledgment.
  - [ ] Mark onboarding as **Completed**.
- [ ] **Step 5: First Day Self-Service Login**
  - [ ] Sign in as the provisioned new employee; verify ESS dashboard and permissions.
- [ ] **Step 6: Probation Evaluation & Confirmation**
  - [ ] Navigate to `/app/probation-evaluation/new` as **HR Maker**.
  - [ ] Select employee, input Performance (1-5), Conduct (1-5), Attendance %, and Recommendation (`Confirm`).
  - [ ] HR Checker performs **`Actions ▼` ➔ `Review`**, then **`Actions ▼` ➔ `Confirm`**.
  - [ ] Verify Employee master stamps **Date of Confirmation** and confirmation log entry.
- [ ] **Step 7: Intern Conversion (Alternative Flow)**
  - [ ] Verify intern conversion API endpoint (`prime_bank_hrms.api.convert_intern_to_employee`) cleanly converts intern to full-time employee without duplicate application overhead.

---

## 📋 Module 06 — Attendance Management & Late Consequences
*Source Document:* `docs/client_presentation/06_Attendance.md`

- [ ] **Step 1: Late Attendance Policy Configuration**
  - [ ] Navigate to `/app/late-attendance-rule` as **HR Maker** (e.g. `Dhaka Branch — Late Policy 2026`).
  - [ ] Set Warning Threshold (`3` lates), Half-Day Threshold (`5` lates), Grace Minutes (`10` min).
  - [ ] Submit for review; **HR Checker** approves via `Actions ▼` ➔ `Approve`.
- [ ] **Step 2: Biometric Device Punch Sync Verification**
  - [ ] Inspect **Employee Checkin** logs (`/app/employee-checkin`) populated via automated device middleware API.
- [ ] **Step 3: Punch Regularization Request (ESS Flow)**
  - [ ] Log in as **Employee**; navigate to `/app/attendance-request/new`.
  - [ ] Submit Attendance Request: Reason `Forgot Punch` (or `Duty Outside Office`), Date range, Explanation.
  - [ ] Employee submits request.
  - [ ] Log in as **HR Checker**; click **`Actions ▼` ➔ `Approve`**; verify attendance entry auto-creates/updates.
- [ ] **Step 4: Manual Attendance Entry (HR Exception Path)**
  - [ ] Navigate to `/app/attendance/new` as **HR Maker**.
  - [ ] Mark Present with `Late Entry = Yes` for an unpunched day; submit attendance.
  - [ ] Verify attribution in Version History (Maker submits directly without checker gate).
- [ ] **Step 5: Automated Late Consequences Engine**
  - [ ] Inspect scheduled job execution (`process_late_attendance`).
  - [ ] Verify generation of **Late Attendance Action** records (Warning notice at 3 lates, Half-Day conversion at 5 lates, salary/leave deduction).
- [ ] **Step 6: Monthly Attendance Review & Analytics**
  - [ ] Inspect **Monthly Attendance Sheet** (`/desk/query-report/Monthly%20Attendance%20Sheet`).
  - [ ] Inspect **Late Attendance Report** and **Overtime Analysis**.

---

## 📋 Module 07 — Leave Management & Entitlements
*Source Document:* `docs/client_presentation/07_Leave.md`

- [ ] **Step 1: Leave Eligibility Policy Configuration**
  - [ ] Navigate to `/app/leave-eligibility-rule/new` as **HR Maker**.
  - [ ] Define eligibility criteria (Company, Grade, Leave Type); Maker submits.
  - [ ] **HR Checker** approves via `Actions ▼` ➔ `Approve`.
- [ ] **Step 2: Balance Check & Leave Application (ESS Flow)**
  - [ ] As **Employee**, navigate to `/app/leave-application/new`.
  - [ ] Select Leave Type; verify live read-only display of **Leave Balance Before Application**.
  - [ ] Pick dates, add reason, and save/submit application.
- [ ] **Step 3: Multi-Level Manager Approval**
  - [ ] Log in as **Line Manager**; inspect application and click **`Approve`**.
  - [ ] Verify ledger deduction and verify presence on **Team Leave Calendar** (`/app/team-leave-calendar`).
- [ ] **Step 4: Leave Without Pay (LWP) with Explicit Consent**
  - [ ] Apply for `Leave Without Pay`; verify consent checkbox and payroll deduction flag.
- [ ] **Step 5: Compensatory Off for Worked Holidays**
  - [ ] File **Compensatory Leave Request** (`/app/compensatory-leave-request/new`) for worked holiday; verify 1 day credited upon approval.
- [ ] **Step 6: Annual Leave Planning & Encashment**
  - [ ] Inspect **Annual Leave Plan** (`/app/annual-leave-plan`) and track planned vs taken days.
  - [ ] Inspect **Leave Encashment** (`/app/leave-encashment/new`).
- [ ] **Step 7: Leave Trend & Absenteeism Analytics**
  - [ ] Run **Leave Trend Analysis** (`/desk/query-report/Leave%20Trend%20Analysis`).
  - [ ] Run **Absenteeism Analysis** report.

---

## 📋 Module 12 — Payroll Month-End Execution
*Source Document:* `docs/client_presentation/12_Payroll_Month_End.md`

- [ ] **Step 1: Setup Components, Structures & Assignments**
  - [ ] Inspect **Salary Components** (`/app/salary-component`): Basic, House Rent Allowance (40%), Festival Bonus, Provident Fund (10%), Welfare Fund (500 BDT), Income Tax.
  - [ ] Verify component fund classifications (`Provident Fund`, `Welfare Fund`).
  - [ ] Inspect **Salary Structure** (`PB Bank Staff Scale 2026`).
  - [ ] Inspect submitted **Salary Structure Assignment** (`/app/salary-structure-assignment`) with computed CTC and Base.
- [ ] **Step 2: Ad-Hoc Earnings & Deductions via Additional Salary**
  - [ ] Create **Additional Salary** (`/app/additional-salary/new`) for staff loan instalment deduction or bonus.
- [ ] **Step 3: Mid-Month Increments & Arrears**
  - [ ] Verify mid-month increment handling with updated assignment and retroactive arrears component.
- [ ] **Step 4: Execute Payroll Entry (The Month-End Run)**
  - [ ] Navigate to `/app/payroll-entry/new` as **HR Maker**.
  - [ ] Select Company (`Prime Bank PLC`), Payroll Frequency (`Monthly`), Posting Date (`2026-09-30`), Start/End Dates.
  - [ ] Click **`Get Employees`**; verify eligible employee list populates.
  - [ ] Click **`Create Salary Slips`**; verify individual salary slips generate in Draft.
  - [ ] Maker submits Payroll Entry for review.
  - [ ] **HR Checker** approves Payroll Entry via **`Actions ▼` ➔ `Approve`** (submits all slips and posts GL entries).
- [ ] **Step 5: Fund Ledgers, Tax Certificates & CBS Bank Advice File**
  - [ ] Verify Provident Fund and Welfare Fund ledger postings.
  - [ ] Inspect **Bank Remittance / CBS File Generation** (`/desk/query-report/Bank%20Remittance%20Report`).
  - [ ] Inspect Salary Register and Tax Deducted at Source (TDS) reports.
- [ ] **Step 6: Employee Self-Service Payslip View**
  - [ ] Log in as Employee; open submitted **Salary Slip** (`/app/salary-slip`).
  - [ ] Verify earnings breakdown, PF deductions, tax withholding, and net pay.

---

## 📋 Module 15 — Separation & Full & Final Settlement
*Source Document:* `docs/client_presentation/15_Separation_and_Settlement.md`

- [ ] **Step 1: Separation Configuration & Clearance Templates**
  - [ ] Inspect **Employee Separation Type** (e.g. `Resignation`, `Retirement`, `Termination`).
  - [ ] Inspect default exit clearance checklists across six departments: HR, IT, Admin, Finance, Security, Business Unit.
- [ ] **Step 2: Record Separation & Notice Period Computation**
  - [ ] Navigate to `/app/employee-separation/new` as **HR Maker**.
  - [ ] Select Employee (`Tabassum Rahman`), Separation Type, Resignation Date.
  - [ ] Verify automatic calculation of Required Notice (`90 days`), Notice Completion Date, Shortfall Days, and Buy-Out Amount.
- [ ] **Step 3: Line Manager Recommendation & Approval Chain**
  - [ ] Line Manager submits **Manager Recommendation** (`/app/manager-recommendation/new`) for separation.
  - [ ] HR Checker accepts recommendation.
  - [ ] HR Maker clicks **`Actions ▼` ➔ `Recommend`** on the Separation record.
  - [ ] HR Checker clicks **`Actions ▼` ➔ `Approve`** (auto-spawns **Exit Clearance** & draft **Full and Final Statement**).
- [ ] **Step 4: Multi-Department Clearance Execution**
  - [ ] Open linked **Exit Clearance** (`/app/exit-clearance`).
  - [ ] Clear items across all 6 departments (HR leave encashment review, IT laptop/email deactivation, Admin assets, Finance loans, Security access card, BU handover).
  - [ ] Set asset recovery amounts where applicable (e.g. damaged laptop BDT 15,000).
  - [ ] Verify clearance flips to `Completed` when all mandatory items are cleared.
- [ ] **Step 5: Mark Cleared & Complete Separation Submit**
  - [ ] On Employee Separation, HR Checker clicks **`Actions ▼` ➔ `Mark Cleared`**.
  - [ ] HR Checker clicks **`Actions ▼` ➔ `Complete`** (Submits separation).
  - [ ] Verify automated cascade in single transaction:
    - Employee status set to `Left`
    - Position relieved and returned to vacancy register
    - User desk login account disabled
    - Alumni record created
- [ ] **Step 6: Full & Final (F&F) Settlement & Exit Interview**
  - [ ] Open linked **Full and Final Statement**; verify auto-pulled receivables (loan balance, notice buyout, asset recovery).
  - [ ] Add payables (Gratuity, Leave Encashment, Festival Bonus); set status to `Settled` and submit.
  - [ ] Inspect **Exit Interview** record (`/app/exit-interview/new`).
