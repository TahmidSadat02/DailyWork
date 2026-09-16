# Sadat — 16 Sept 2026 Testing Todo List
**Stories to Test Today:** Story by Business 07 to 12  
**System Target:** `http://localhost:8080` (Docker stack `primebank.local`)  
**Methodology:** Slow-Mo Visual Browser Walkthrough with Glowing Element Outlines, On-Screen Banners, and Maker–Checker Verification.

---

## Testing Environment & Credentials

| Role | Email / Username | Password | Notes |
| :--- | :--- | :--- | :--- |
| **System Administrator** | `Administrator` | `Prime@2026` / `admin` | Full system setup, schemas, bench migrations |
| **HR Maker** | `maker@primebank.com.bd` | `Prime@2026` | Drafts rules, allocations, entries. *(Note: Restricted to Company `UniSoft`)* |
| **HR Checker** | `checker@primebank.com.bd` | `Prime@2026` | Approves rules, loans, payroll entries via `Actions ▼` |
| **Employee (ESS)** | Linked employee email | `Prime@2026` | Applications, self-service claims, appraisals, slips |
| **Line / Branch Manager** | Manager email | `Prime@2026` | Reviews, leave approvals, evaluations |

> [!IMPORTANT]
> **Key Frappe Desk UI Rule:**
> In Frappe Desk v15, workflow transitions (`Submit for Review`, `Approve`, `Reject`) do **not** appear as standalone navbar buttons. They are nested inside the top-right **`Actions ▼`** dropdown button (`.actions-btn-group button`).

---

## 📋 Story 07 — Leave Management
*Source Document:* `docs/story_by_business/07_Leave.md`

- [ ] **Step 1: Configure Entitlement Rule (`Leave Eligibility Rule`)**
  - [ ] Log in as **HR Maker** (`maker@primebank.com.bd`).
  - [ ] Navigate to `/app/leave-eligibility-rule/new`.
  - [ ] Set Rule Name, Leave Type (`Casual Leave`), Effective From (`2026-01-01`), Company (`UniSoft`), Criteria (`Grade`), Value (`Grade F`), Action (`Deny`).
  - [ ] Click **Save**, then click **`Actions ▼`** ➔ **`Submit for Review`** (Draft ➔ Submitted).
  - [ ] Log in as **HR Checker** (`checker@primebank.com.bd`), open the rule, click **`Actions ▼`** ➔ **`Approve`** (Submitted ➔ Approved).
- [ ] **Step 2: Balance Verification & Leave Application (ESS)**
  - [ ] Log in as **Employee** (or HR Maker in ESS mode).
  - [ ] Navigate to `/app/leave-application/new`.
  - [ ] Select Employee & Leave Type (`Casual Leave`); verify that **Leave Balance Before Application** is displayed live and read-only.
  - [ ] Enter From / To dates, add reason, and click **Save** (Status: *Open*).
- [ ] **Step 3: Manager Approval & Calendar Reflection**
  - [ ] Log in as **Line Manager / Approver**.
  - [ ] Open the pending Leave Application and click **Approve**.
  - [ ] Verify balance deduction and verify entry on **Team Leave Calendar** (`/app/team-leave-calendar`).
- [ ] **Step 4: Explicit Leave Without Pay (LWP)**
  - [ ] Submit a Leave Application with Leave Type = `Leave Without Pay`.
  - [ ] Verify validation allows application and flags LWP explicitly for payroll deduction.
- [ ] **Step 5: Compensatory Leave Request**
  - [ ] Navigate to `/app/compensatory-leave-request/new`.
  - [ ] Apply for worked holiday credit; verify approver sign-off adds 1 day to balance.
- [ ] **Step 6: Annual Leave Plan & Encashment**
  - [ ] Check `/app/annual-leave-plan` and verify Taken Days auto-update.
  - [ ] Test `/app/leave-encashment/new` form rendering.
- [ ] **Step 7: Analytics Reports**
  - [ ] Verify `/desk/query-report/Leave%20Trend%20Analysis`.
  - [ ] Verify `/desk/query-report/Leave%20Type%20Usage%20Analysis`.
  - [ ] Verify `/desk/query-report/Absenteeism%20Analysis`.

---

## 📋 Story 08 — Benefits, Loans, and Claims
*Source Document:* `docs/story_by_business/08_Benefits_Loans_and_Claims.md`

- [ ] **Step 1: Benefit Plan & Eligibility Definition**
  - [ ] Navigate to `/app/benefit-plan/new` as HR Maker.
  - [ ] Configure Benefit Plan (e.g. Executive Health Scheme, Optical, Dental).
  - [ ] Define eligibility criteria (Grades / Service Tenure) and submit for review; approve as Checker.
- [ ] **Step 2: Employee Benefit Enrollment**
  - [ ] Navigate to `/app/benefit-enrollment/new`.
  - [ ] Enroll employee into active Benefit Plan; confirm coverage limits and effective dates.
- [ ] **Step 3: Benefit Claim Submission & Partial Approval**
  - [ ] Navigate to `/app/employee-benefit-claim/new` as Employee.
  - [ ] Submit claim with bill amount, invoice date, and receipt attachment.
  - [ ] Log in as HR Approver: verify partial approval amount feature (e.g. Claim: 5,000 BDT, Approved: 3,500 BDT).
- [ ] **Step 4: Medical Network & Insurance**
  - [ ] Check `/app/medical-network-hospital` list view and verify hospital search.
  - [ ] Verify `/app/insurance-policy` policy number and coverage links.
- [ ] **Step 5: Staff Loan with Payroll Recovery**
  - [ ] Navigate to `/app/staff-loan-application/new` (e.g. Car / House Building / Personal Loan).
  - [ ] Set Principal Amount, Repayment Period (months), and Interest Rate.
  - [ ] Checker approves loan ➔ verify auto-generated **Loan Repayment Schedule**.
- [ ] **Step 6: Welfare Fund Assistance**
  - [ ] Submit `/app/welfare-fund-application/new` for emergency grant.
  - [ ] Verify Welfare Committee approval workflow.
- [ ] **Step 7: Festival Bonus & Benefit Statement**
  - [ ] Verify `/app/festival-bonus-run/new` configuration.
  - [ ] View individual **Employee Benefit Statement** summary.

---

## 📋 Story 09 — Performance Management
*Source Document:* `docs/story_by_business/09_Performance_Management.md`

- [ ] **Step 1: Appraisal Cycle & Template Setup**
  - [ ] Navigate to `/app/appraisal-cycle/new` as HR Admin / Maker.
  - [ ] Set Cycle Name (e.g. `Annual Appraisal 2026`), Start / End dates, and Evaluation Period.
  - [ ] Link `/app/appraisal-template` with KPI weights and Competency criteria.
- [ ] **Step 2: Goal Setting & Tracking**
  - [ ] Log in as Employee: create personal goals under `/app/goal/new`.
  - [ ] Set targets, metric type, weightage, and link to corporate objectives.
  - [ ] Line Manager reviews and confirms goals.
- [ ] **Step 3: Component Scoring (Self & 360 Feedback)**
  - [ ] Complete **Self Appraisal** ratings and achievements comments.
  - [ ] Collect Peer / Subordinate **Feedback 360** inputs.
- [ ] **Step 4: Manager Appraisal Review & Submission**
  - [ ] Line Manager opens Employee Appraisal form (`/app/appraisal`).
  - [ ] Score each KPI and competency; calculate system weighted score.
  - [ ] Submit review via `Actions ▼` ➔ `Submit for Review`.
- [ ] **Step 5: Performance Calibration**
  - [ ] Navigate to `/app/performance-calibration`.
  - [ ] Verify Bell Curve normalization and Grade distribution.
- [ ] **Step 6: PIP & Individual Development Plan (IDP)**
  - [ ] Test `/app/performance-improvement-plan/new` for low ratings (< 2.5).
  - [ ] Test `/app/individual-development-plan/new` for high-potential growth tracks.

---

## 📋 Story 10 — Learning and Development (L&D)
*Source Document:* `docs/story_by_business/10_Learning_and_Development.md`

- [ ] **Step 1: Course Catalog & Annual Training Budget**
  - [ ] Create Training Course (`/app/course/new`) and define prerequisites / syllabus.
  - [ ] Configure `/app/training-program` and link annual department training budget.
- [ ] **Step 2: Training Needs Assessment (TNA) & Calendar**
  - [ ] Verify TNA aggregation from Appraisal IDPs.
  - [ ] Check Annual Training Calendar view (`/app/training-calendar`).
- [ ] **Step 3: Schedule Training Event & QR Attendance**
  - [ ] Navigate to `/app/training-event/new`.
  - [ ] Assign Trainer, Room / Online link, Event Dates, and Maximum Seats.
  - [ ] Inspect QR code generation for digital session attendance.
- [ ] **Step 4: Training Nomination & Approval Chain**
  - [ ] Employee submits `/app/training-request/new`.
  - [ ] Manager approves request ➔ Seat confirmed in Training Event.
- [ ] **Step 5: Post-Training Assessment & Certificate Issuance**
  - [ ] Record trainer evaluation scores (`/app/training-result`).
  - [ ] Submit participant feedback survey.
  - [ ] Verify digital certificate generation and storage on Employee profile.
- [ ] **Step 6: Compliance & Mandatory Training Reports**
  - [ ] Verify `/desk/query-report/Mandatory%20Training%20Compliance%20Report` (e.g. AML/CFT, Cyber Security).

---

## 📋 Story 11 — Employee Relations & Disciplinary
*Source Document:* `docs/story_by_business/11_Employee_Relations.md`

- [ ] **Step 1: Employee Grievance Submission**
  - [ ] Log in as Employee: navigate to `/app/employee-grievance/new`.
  - [ ] Fill grievance category, subject, narrative, and requested resolution; click **Save**.
- [ ] **Step 2: HR Investigation & Resolution**
  - [ ] Log in as HR Maker / Committee Head: open grievance record.
  - [ ] Log investigation notes, witness testimonies, and mediation hearings.
  - [ ] Record resolution status and notify grievance filer.
- [ ] **Step 3: Anonymous Whistleblower Portal**
  - [ ] Test `/app/whistleblower-report/new` with anonymous toggle enabled.
  - [ ] Verify identity protection: Employee ID remains hidden/null.
- [ ] **Step 4: Disciplinary Action & Show Cause Lifecycle**
  - [ ] Navigate to `/app/disciplinary-action/new`.
  - [ ] Issue formal **Show Cause Notice** with deadline (e.g. 7 days).
  - [ ] Record employee explanation response.
  - [ ] Constitute Inquiry Committee and record penalty / exoneration findings.
- [ ] **Step 5: Conflict of Interest (COI) Declaration**
  - [ ] Open `/app/conflict-of-interest-declaration/new`.
  - [ ] Submit annual external directorship / family financial ties declaration.
- [ ] **Step 6: Employee Wellbeing & Satisfaction Surveys**
  - [ ] Inspect `/app/employee-satisfaction-survey`.
  - [ ] Review `/app/employee-recognition` / Award nomination workflow.

---

## 📋 Story 12 — Payroll Month-End Run
*Source Document:* `docs/story_by_business/12_Payroll_Month_End.md`

- [ ] **Step 1: Salary Components & Structure Assignment**
  - [ ] Verify `/app/salary-component` (Basic, House Rent, Medical, Conveyance, PF, Tax).
  - [ ] Verify `/app/salary-structure` with mathematical formula calculations.
  - [ ] Check `/app/salary-structure-assignment` for active test employees.
- [ ] **Step 2: Additional Salary (Ad-hoc Allowances / Deductions)**
  - [ ] Navigate to `/app/additional-salary/new`.
  - [ ] Add performance bonus or recovery deduction for the current payroll cycle.
- [ ] **Step 3: Mid-Month Increments & Proration**
  - [ ] Verify salary increment effective date split behavior.
- [ ] **Step 4: Payroll Entry Execution (Month-End Run)**
  - [ ] Navigate to `/app/payroll-entry/new`.
  - [ ] Set Company (`UniSoft` / `Prime Bank PLC`), Payroll Frequency (`Monthly`), Posting Date.
  - [ ] Click **Get Employees** ➔ Confirm list of eligible employees.
  - [ ] Click **Create Salary Slips** ➔ Confirm automated calculation including attendance & LWP deductions.
  - [ ] Submit Payroll Entry via `Actions ▼` ➔ `Submit for Review` ➔ Checker `Approve`.
- [ ] **Step 5: Bank Transfer File (CBS) & Statutory Reports**
  - [ ] Verify export of CBS Electronic Fund Transfer File (BEFTN / NPSB format).
  - [ ] Verify `/desk/query-report/Monthly%20Tax%20Deduction%20Report`.
  - [ ] Verify Provident Fund (PF) and Gratuity deduction statements.
- [ ] **Step 6: Employee Salary Slip Verification (ESS)**
  - [ ] Log in as Employee: navigate to `/app/salary-slip`.
  - [ ] Inspect generated payslip: check Basic, allowances, PF, tax, net pay, and PDF print preview.

---

## Daily Target & Wrap-Up
- [ ] Complete test execution for Stories 07, 08, 09, 10, 11, 12.
- [ ] Document all pass/fail findings, field name discrepancies, and workflow transitions.
- [ ] Compile final results into `sadat_report_16sept.md` at the end of the day.
