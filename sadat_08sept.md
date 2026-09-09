# Testing Summary: Modules 01, 02, 08, 09, 10, 11
**Target Site:** `http://172.17.15.141:8000`
**Date:** 2026-09-08

---

### Doc 01: Recruitment (Requisition to Offer)
- [x] **1. Vacant Position Check:** Pass - Confirmed vacant POS-2026-00007 (Teller).
- [x] **2. Raise Job Requisition:** Pass - Created HR-HIREQ-00002 linked to vacant position.
- [x] **3. Approve Requisition:** Pass - Maker Checked, Checker Approved (Open & Approved).
- [x] **4. Publish Job Opening:** Pass - Created HR-OPN-2026-0004 with pre-screening questions.
- [x] **5. Capture Job Applicant:** Pass - Candidate Tanvir Ahmed recorded; duplicate questions generated.
- [x] **6. Public Application Tracking:** Pass - Tracking works via token link (not email search).
- [x] **7. Screen & Shortlist:** Pass - Auto-computed screening pass; candidate shortlisted.
- [x] **8. Interview Panel:** Partial - Interview scheduled; panel feedback is non-functional stub.
- [x] **9. Issue Job Offer:** Partial - Offer created and accepted; HR Maker cannot submit offer.

---

### Doc 02: Onboarding (Offer to First Morning)
- [x] **1. Onboarding Template:** Pass - Activities configured; document checklist configured separately.
- [x] **2. Create Employee Onboarding:** Pass - Onboarding record created and linked to accepted offer.
- [x] **3. Employee Master & Position Assignment:** Partial - Employee created; duplicate email allowed; future date delays fill flip.
- [x] **4. Complete Onboarding Checklist:** Pass - Onboarding tasks generated and completed; no activity checkboxes.
- [x] **5. Candidate First Login:** Fail - Joiner user account not auto-provisioned on hire.
- [x] **6. Probation Evaluation:** Partial - Confirmation flow works; Apply Extension throws TypeError.

---

### Doc 08: Benefits, Loans and Claims
- [x] **1. Benefit Plan Enrolment:** Pass - Enrolled employee into schemes with automatic eligibility validation.
- [x] **2. Expense & Medical Claims:** Pass - Claim submission verified; partial approval supported with variance log.
- [x] **3. Staff Loan & Auto-Recovery:** Pass - Approved staff loan SLOAN-2026-128974 with 12-month salary deduction schedule.
- [x] **4. Empanelled Hospital Network:** Pass - Directory queried and insurance coordination data verified.
- [x] **5. Welfare Assistance:** Pass - Emergency relief application and disbursement verified.
- [x] **6. Benefit Statement:** Pass - Consolidated statement BSTMT-2026-128975 generated (loans, PF, insurance, claims).

---

### Doc 09: Performance Management
- [x] **1. Policy & Appraisal Cycles:** Pass - Configured appraisal cycle, KPI weightings, and rating scales.
- [x] **2. Goal Tracking & Check-ins:** Pass - Verified real-time KRA progress updates and manager check-in notes.
- [x] **3. Self & Manager Review:** Pass - Final score consolidates and locks into tamper-proof Submitted state.
- [x] **4. 360-Degree Feedback:** Partial - Peer feedback collected; 360 Feedback Summary report throws ModuleNotFoundError.
- [x] **5. Committee Calibration & Scorecard:** Pass - Computed 90% weighted scorecard SCORECARD-2026-128978.
- [x] **6. PIP & Performance Reports:** Partial - Active PIP tracking works; Appraisal Status Report throws SQL error.

---

### Doc 10: Promotion
- [x] **1. Policy & Vacancy Gates:** Pass - Verified rules blocking promotion to occupied seats or active disciplinary cases.
- [x] **2. Manager Recommendation:** Pass - Recommendation submitted with appraisal history and conduct record.
- [x] **3. Draft Promotion Case:** Pass - System eligibility engine ran and validated vacant position requirement.
- [x] **4. Committee Review & Decision:** Pass - Processed through promotion committee review and interview scoring.
- [x] **5. Apply Promotion Handoff:** Pass - Promotion PROM-2026-105503 auto-created Increment INCR-2026-105505 and Employee Promotion HR-EMP-PRO-2026-00002.
- [x] **6. Promotion Letter:** Pass - Official promotion letter generated with updated designation, grade, and compensation.

---

### Doc 11: Transfer and Posting
- [x] **1. Target Seat Validation:** Pass - Prevented moves to unapproved or non-vacant positions.
- [x] **2. Releasing & Receiving Sign-offs:** Pass - Handshake approval chain between branch managers and HR Checker executed.
- [x] **3. Mandatory Handover Gate:** Pass - Hard stop on HANDOVER-2026-128992 refused transfer until keys and access were signed off.
- [x] **4. Move Execution:** Pass - Move executed (TRF-2026-108345) and auto-submitted Employee Transfer HR-EMP-TRN-2026-00004.
- [x] **5. Relocation Claims & Orders:** Pass - Relocation claim approved RELOC-2026-108354 and Posting Order printed.
- [x] **6. Transfer Reporting:** Partial - Transfer audit trail verified; Pending Transfers Report throws Python TypeError.

---

### Key Defects and Issues
1. **Interview Feedback (Doc 01):** Panel scoring is a static HTML stub with no input form.
2. **Offer Submission (Doc 01):** HR Maker lacks permission to submit Job Offer.
3. **Duplicate Employee Gate (Doc 02):** No duplicate-email validation when creating Employee.
4. **Joiner Auto-Provisioning (Doc 02):** User accounts not auto-created on hire.
5. **Probation Extension (Doc 02):** Apply Extension throws TypeError missing argument 'name'.
6. **360 Feedback Summary Report (Doc 09):** Fails with ModuleNotFoundError due to path mismatch.
7. **Appraisal Status Report (Doc 09):** SQL error querying non-existent 'status' column.
8. **Competency Gap Report (Doc 09):** SQL error querying non-existent 'department' column.
9. **Pending Transfers Report (Doc 11):** TypeError comparing datetime.date with string today().
