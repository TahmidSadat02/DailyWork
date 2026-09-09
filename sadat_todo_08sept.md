# Testing Todo List: Full Lifecycle (Docs 01 to 16)
**Target Site:** `http://172.17.15.141:8000`
**Date:** 2026-09-08

---

### Doc 01: Recruitment (Requisition to Offer)
- [ ] **1. Vacant Position Check:** Confirm a `Vacant` position exists (`POS-2026-xxxxx`).
- [ ] **2. Raise Job Requisition:** Create requisition linked to the vacant position (`HR-HIREQ-xxxxx`).
- [ ] **3. Approve Requisition:** Maker-Checker approval (`Check` -> `Approve` / `Compliance Review` if critical).
- [ ] **4. Publish Job Opening:** Set job details + configure pre-screening knock-out questions.
- [ ] **5. Capture Job Applicant:** Add candidate record with name, email & resume.
- [ ] **6. Public Application Tracking:** Verify status tracking via `/track_application`.
- [ ] **7. Screen & Shortlist:** Filter `Screening Passed = Yes` and mark candidate `Shortlisted`.
- [ ] **8. Interview Panel:** Schedule interview round, submit panel feedback & evaluation score.
- [ ] **9. Issue Job Offer:** Generate Job Offer -> Candidate accepts (`Status = Accepted`).

---

### Doc 02: Onboarding (Offer to First Morning)
- [ ] **1. Onboarding Template:** Setup activities (IT, ID card, desk) & required document checklist.
- [ ] **2. Create Employee Onboarding:** Link accepted `Job Applicant` to onboarding template.
- [ ] **3. Employee Master & Position Assignment:** Create Employee & submit `Primary` Position Assignment.
- [ ] **4. Complete Onboarding Checklist:** Mark boarding tasks done, collect required docs & Code of Conduct dates.
- [ ] **5. Candidate First Login:** Verify newly provisioned employee login access.
- [ ] **6. Probation Evaluation:** Draft & approve `Probation Evaluation` (`Confirm`/`Extend`) & check `Confirmation Due Report`.

---

### Doc 03: Position and Headcount Management
- [ ] **1. Org Masters Verification:** Verify Zone, Region, Business Unit, Branch, Department, Designation & Grade.
- [ ] **2. Create Position & Cycle Gate:** Create Position, verify naming/grade auto-fetch, test `reports_to` cycle refusal.
- [ ] **3. Position Request Maker-Checker:** Draft `Position Request` -> `Check` -> `Approve` (or `Compliance Review`).
- [ ] **4. Fill & Relieve Assignments:** Test Primary, Acting, and Additional assignments; test seat hold and Relieve action.
- [ ] **5. Vacancy Register & Dashboards:** Run `Vacancy Report`, `Filled vs Vacant`, `Headcount Variance`, and Position Dashboard.
- [ ] **6. Regulatory Manpower & Abolish Gate:** Run `Regulatory Manpower Report` (Conventional & Islamic); test abolish gate on filled post.

---

### Doc 04: Employee Master Data and Privacy
- [ ] **1. Create / Extend Employee Record:** Populate statutory banking fields, NID, TIN, passport & dual citizenship flags.
- [ ] **2. Family, Education & Skills:** Add child table entries for dependents, emergency contacts, qualifications & licenses.
- [ ] **3. Document Expiry Tracking:** Upload employee documents and verify expiry date alerting logic.
- [ ] **4. HR Maker-Checker Correction:** Submit `Employee Change Request` via HR Maker and approve via HR Checker.
- [ ] **5. ESS Profile Update Request:** Log in as Employee and submit profile data correction via Self Service.
- [ ] **6. Audit Trail Verification:** Inspect Version History and field-level change logs for sensitive attribute updates.

---

### Doc 05: Attendance (Capture to Correction)
- [ ] **1. Configure Late Attendance Rule:** Set grace periods, late thresholds, and deduction penalty linkages.
- [ ] **2. Capture Check-ins:** Generate raw `Employee Checkin` logs (IN/OUT) and verify device mapping.
- [ ] **3. Missed Punch Regularisation:** Submit `Attendance Request` for missing log and approve via reporting manager.
- [ ] **4. Manual Attendance Marking:** HR Maker bulk-marks manual attendance for off-site or exception staff.
- [ ] **5. Late Deduction Processing:** Execute scheduled late-attendance penalty job and inspect deduction outputs.
- [ ] **6. Attendance Summaries & Reports:** Review `Monthly Attendance Sheet` and shift exception reports.

---

### Doc 06: Leave (Apply to Encashment)
- [ ] **1. Configure Entitlement Rules:** Set up `Leave Policy`, `Leave Allocation`, and compensatory off credit rules.
- [ ] **2. Leave Balance Verification:** Check available leave balances via Employee Self Service.
- [ ] **3. Apply for Leave:** Submit `Leave Application` with mandatory attachments and reason.
- [ ] **4. Manager & Delegate Approval:** Approve application via reporting manager or delegated authority.
- [ ] **5. Leave Without Pay (LWP):** Submit explicit LWP with salary deduction consent flag.
- [ ] **6. Compensatory Off Claim:** Claim compensatory leave for holiday duty and verify balance credit.
- [ ] **7. Annual Leave Plan & Encashment:** File annual leave schedule and process leave encashment application.
- [ ] **8. Leave Ledger & Reports:** Reconcile `Leave Ledger` against payroll deduction inputs.

---

### Doc 07: Payroll Month-End
- [ ] **1. Components & Salary Structure:** Verify earnings, deductions, formula components, and active assignments.
- [ ] **2. Process Additional Salary:** Create one-off bonuses, overtime, claims, or ad-hoc deductions.
- [ ] **3. Mid-Month Salary Increment:** Submit amended `Salary Structure Assignment` with mid-cycle effective date.
- [ ] **4. Execute Payroll Entry:** Create, calculate, and submit monthly `Payroll Entry` -> generate `Salary Slip` batch.
- [ ] **5. Banking & Tax CBS Exports:** Export CBS direct-credit payment advice file and generate monthly tax statements.
- [ ] **6. Employee Payslip Access:** Log in as Employee and verify payslip visibility, breakdown, and PDF download.

---

### Doc 08: Benefits, Loans and Claims
- [ ] **1. Benefit Plan Setup:** Configure benefit plans with grade/designation eligibility limits.
- [ ] **2. Employee Plan Enrolment:** Enrol employee into hospitalization, outpatient, and life cover policies.
- [ ] **3. Expense & Medical Claims:** Submit reimbursement claim with bills -> Manager review -> HR approval.
- [ ] **4. Empanelled Hospital Network:** Query hospital directory and verify cashless/insurance coordination data.
- [ ] **5. Staff Loan Processing:** Apply for Staff Loan -> Committee review -> Loan Disbursement -> EMI schedule generation.
- [ ] **6. Welfare Assistance:** Submit emergency welfare relief application and review disbursement flow.
- [ ] **7. Festival Bonus & Statements:** Run seasonal festival bonus batch and verify Total Rewards Statement.

---

### Doc 09: Performance Management
- [ ] **1. Appraisal Policy & Cycle Setup:** Open annual appraisal cycle, define KPI weightings and rating scales.
- [ ] **2. Goal Sheet & Progress Update:** Set annual KRAs/KPIs for employee; update milestone progress mid-year.
- [ ] **3. Appraisal Self & Manager Review:** Employee submits self-appraisal -> Manager submits performance score.
- [ ] **4. 360-Degree & Competency Feedback:** Collect multi-rater peer and subordinate review evaluations.
- [ ] **5. Periodic Check-in Logs:** Record scheduled one-on-one quarterly review notes and coaching agreements.
- [ ] **6. Committee Calibration:** Run rating normalization matrix and apply forced-distribution curve.
- [ ] **7. PIP & IDP Initiation:** Initiate Performance Improvement Plan or Individual Development Plan for outliers.
- [ ] **8. Performance Analytics Dashboard:** Inspect company-wide appraisal distribution and completion ratios.

---

### Doc 10: Promotion
- [ ] **1. Promotion Policy & Cycle:** Define minimum tenure, grade jumps, and open the annual promotion window.
- [ ] **2. Manager Nomination:** Manager submits promotion nomination supported by appraisal ratings and clean conduct.
- [ ] **3. Draft Promotion Case:** Run system eligibility engine, verify vacant position requirement, and draft case.
- [ ] **4. HR Maker-Checker Review:** Review eligibility criteria, salary proposal, and forward to committee.
- [ ] **5. Promotion Committee Action:** Record committee interview scores, quota checks, and formal committee sign-off.
- [ ] **6. Promotion Letter & Master Update:** Issue official promotion letter -> Auto-update Employee Grade, Designation & Salary.

---

### Doc 11: Transfer and Posting
- [ ] **1. Draft Transfer Case:** Create transfer case (branch-to-branch or division-to-division); verify vacant post.
- [ ] **2. Workflow Sign-offs:** Releasing Manager -> Receiving Manager -> HR Checker approval chain.
- [ ] **3. Handover & Clearance Gate:** Releasing branch verifies department clearance, file handover, and asset release.
- [ ] **4. Apply Transfer:** Submit transfer execution -> Auto-update Employee branch, department & position assignment.
- [ ] **5. Relocation Claims & Orders:** Generate formal Transfer Order letter and process relocation expense claims.
- [ ] **6. Branch Staffing Reconciliation:** Verify branch headcount balance and transfer audit history.

---

### Doc 12: Learning and Development
- [ ] **1. Training Catalog & Budget:** Define internal/external training courses, competencies, and annual training budget.
- [ ] **2. Training Needs Analysis (TNA):** Collate manager training requests and generate annual training calendar.
- [ ] **3. Schedule Training Event:** Create training session, allocate instructor/venue, and generate QR attendance code.
- [ ] **4. Enrollment & Approval:** Employee requests course enrollment -> Manager approval -> Batch confirmation.
- [ ] **5. Assessment & Certification:** Conduct post-training evaluation, record scores, and issue certificate with verification URL.
- [ ] **6. Regulatory Training Compliance:** Generate Bangladesh Bank mandatory training compliance report.

---

### Doc 13: Employee Relations
- [ ] **1. Lodge Grievance:** Employee files confidential grievance via self-service or direct Desk submission.
- [ ] **2. Grievance Investigation & Resolution:** Assign investigator, record findings, issue decision, and close case.
- [ ] **3. Anonymous Whistleblowing:** Submit report via public `/whistleblow` endpoint and verify secure tracker.
- [ ] **4. Disciplinary Proceedings:** Issue Show Cause Notice -> Receive explanation -> Inquiry committee -> Formal sanction.
- [ ] **5. Conflict of Interest (COI):** Submit annual COI declaration and route positive declarations for compliance review.
- [ ] **6. Health & Safety Incident Reporting:** Log workplace incident or hazard and track corrective action plan.

---

### Doc 14: Separation and Settlement
- [ ] **1. Separation Policy & Checklist:** Configure notice rules per employee type and departmental clearance checklist.
- [ ] **2. Resignation Submission:** Employee submits resignation on portal; verify automated notice period calculation.
- [ ] **3. Manager Review & Acceptance:** Line manager records recommendation -> HR confirms official last working day.
- [ ] **4. Departmental Clearances:** Complete six required sign-offs (IT, Admin, Branch, Finance, HR, Legal).
- [ ] **5. Final Clearance Sign-off:** Verify all clearance tickets are closed and marked Cleared.
- [ ] **6. Final Settlement (F&F):** Compute gratuity, PF, leave encashment, pending claims, and approve settlement statement.
- [ ] **7. Release Letters & De-provisioning:** Issue Service Certificate & Release Order; verify user account deactivation.

---

### Doc 15: Alumni Engagement
- [ ] **1. Activate Alumni Record:** Transition separated employee into Alumni database upon completed clearance.
- [ ] **2. Consent Tracking:** Capture explicit data retention and communication consent per privacy policy.
- [ ] **3. Alumni Portal Provisioning:** Send portal invitation and test login access at `/alumni`.
- [ ] **4. Alumni Self-Service:** Update personal contact details, download tax/experience documents, view vacancies.
- [ ] **5. Alumni Communications:** Send newsletter/event broadcast respecting opted-in consent filters.
- [ ] **6. Boomerang Rehire:** Initiate fast-track re-application for alumnus and link historical records.
- [ ] **7. Data Erasure Request:** Test right-to-be-forgotten request and verify privacy data purge/anonymization.

---

### Doc 16: Identity, Access and Audit
- [ ] **1. Audit Trail Inspection:** Review immutable Version History on governed doctypes and verify field diffs.
- [ ] **2. Maker-Checker Verification:** Verify strict role separation (maker cannot approve own submission) on key workflows.
- [ ] **3. Automated Joiner Provisioning:** Confirm User creation, employee linkage, and default role assignment on hire.
- [ ] **4. Automated Leaver Deprovisioning:** Confirm immediate User deactivation and session termination upon separation.
- [ ] **5. Role-Based Field Masking:** Verify restricted/salary fields are hidden or read-only for unauthorized roles.
- [ ] **6. Temporary Delegation & Revert:** Configure approval delegation with defined end-date and verify auto-expiry.
