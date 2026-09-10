# Daily QA & End-to-End Testing Report

**Author / Tester:** Sadat  
**Date:** 10 September 2026  
**Project:** Prime Bank HRMS ERP (`prime_bank_hrm_erp-redev`)  
**Target Site:** `http://localhost:8080` (Site: `primebank.local`, Backend: `prime-bank-hrms-backend-1`)  
**Test Approach:** Automated Browser Testing (Playwright Chromium, headed desktop execution, `slowMo`, live role-switching) + Backend RPC verification (`bench execute`)  
**Overall Result:** **33 / 33 Test Steps Passed (100% Pass Rate across 5 Core Business Stories)**

---

## 1. Executive Summary

Today, I executed comprehensive, headed browser end-to-end testing across the first five core modules of the Prime Bank HRMS platform per the official business story runbooks (`docs/story_by_business/01` through `05`). 

All test scenarios were validated directly in the Desk UI using dedicated, role-segregated test personas (`maker@primebank.com.bd`, `checker@primebank.com.bd`, employee self-service accounts, and public candidates).

### Summary Table

| # | Business Story / Module | Runbook Document | Test Roles | Steps | Result |
|:---:|:---|:---|:---|:---:|:---:|
| **01** | **Position & Headcount Management** | `01_Position_and_Headcount.md` | HR Maker, HR Checker, Auditor | 6 / 6 | **100% PASS** |
| **02** | **Identity, Access & Audit** | `02_Identity_Access_and_Audit.md` | HR Maker, HR Checker, ESS, Admin | 6 / 6 | **100% PASS** |
| **03** | **Recruitment & Talent Acquisition** | `03_Recruitment.md` | HR Maker, Checker, Candidate | 9 / 9 | **100% PASS** |
| **04** | **Onboarding (Offer to First Morning)** | `04_Onboarding.md` | HR Maker, HR Checker, Employee | 6 / 6 | **100% PASS** |
| **05** | **Employee Master Data & Privacy** | `05_Employee_Master_Data_and_Privacy.md` | HR Maker, HR Checker, Employee | 6 / 6 | **100% PASS** |
| **Total** | **All 5 Business Domains** | — | — | **33 / 33** | **100% PASS** |

---

## 2. Detailed Work Completed Today

### Module 01: Position & Headcount Management
- **Org Structure Masters:** Created and verified complete organisational hierarchy: Zone (`Dhaka Zone`) $\to$ Region (`Dhaka Region`) $\to$ Business Unit (`Retail Banking`) $\to$ Branch (`Dhaka Main`, Conventional) $\to$ Department (`Retail Operations - Co.`), Designation (`Customer Service Officer`), and Grade (`Grade 3`).
- **Independent Position Master:** Created `POS-2026-00001` (*Cards Operations Officer — Dhaka*) existing independent of incumbents, capacity of 1, status `Vacant`, `vacant_since` auto-stamped, and append-only `Position Log` event recorded.
- **Maker-Checker Position Request:** Tested structural change workflow on `PR-2026-00003` (capacity: 2). Maker clicked `Check`; verified in UI and engine that Maker self-approval is strictly forbidden (BR-19); HR Checker approved (`docstatus = 1`), automatically materializing `POS-2026-00014`.
- **Fill & Relieve Lifecycle:** Assigned employee `HR-EMP-00023` (*Tanvir Ahmed*) via `EPA-2026-00016` (status flipped to `Filled`); relieved assignment with reason `"Transfer"` (status flipped back to `Vacant` and `vacant_since` re-stamped).
- **Establishment & Vacancy Reporting:** Successfully generated `Vacancy Report` (displaying open posts with aging buckets `0-30 days`), `Filled vs Vacant Positions`, and `Headcount Variance Analysis`.
- **Bangladesh Bank Regulatory Manpower Return:** Successfully generated statutory `Regulatory Manpower Report` and `Organization Structure Report` directly from current database records without spreadsheets.

### Module 02: Identity, Access & Audit
- **Immutable Audit Trail:** Opened and verified the `Version History` dialog on Employee `HR-EMP-00001` (*Tanvir Ahmed*) displaying before/after diffs and actor timestamps; confirmed `Position Log` list view is strictly append-only with no delete or manual add actions.
- **Four-Eyes Segregation (BR-19):** Walked `Employee Change Request` `ECR-2026-00010`. Maker drafted and applied `Check`; `Approve` button was completely hidden for Maker; server-side execution attempt was rejected (`DENIED: Permission Error`); HR Checker approved and applied changes to master.
- **Automatic Provisioning on Hire:** Hired new employee `HR-EMP-00009` (*Farhana Yasmin*); confirmed desk User account (`farhana.hire...@primebank.com.bd`) was auto-provisioned in the same database transaction with roles `["Employee Self Service", "Employee"]` and linked to `Employee.user_id`.
- **Immediate De-Provisioning on Exit:** Submitted `Employee Separation` `HR-EMP-SEP-2026-00003`; confirmed the linked User account was immediately disabled (`enabled: 0`) in the same transaction.
- **Field-Level Privacy (`permlevel: 1`):** Verified sensitive identifiers (National ID, TIN, e-TIN, Birth Reg) are visible to HR Checker, but returned as `null` / blank when viewed by an Employee Self Service user (`ess@primebank.com.bd`); cross-record employee access denied.
- **Approval Delegation:** Tested `Approval Delegation` (`DELEG-2026-00018` Active status; `DELEG-2026-00019` past window auto-reverting to Expired).

### Module 03: Recruitment & Talent Acquisition
- **Vacancy Verification:** Confirmed vacant position `POS-2026-00001` on establishment register prior to raising requisition.
- **Requisition Maker-Checker:** Maker raised `HR-HIREQ-00006` (`35,000 BDT`, 1 vacancy); Maker applied `Check`; Checker approved to `"Open & Approved"`.
- **Job Opening & Pre-Screening:** Published opening `HR-OPN-2026-0007` with knockout questions (*Minimum education: Bachelor's [Knock-out: Yes]*, *Bangla typing speed: 30 wpm*).
- **Candidate Auto-Screening & Token:** Captured candidate `tanvir.candidate...`; evaluated responses (`custom_screening_passed = 1`); generated secure UUID status token (`GpXXsQWF46qhXcdW6XVPkw`).
- **Public Tracking Portal (`/track_application`):** Verified tokenless access is denied; valid token rendered candidate header, opening title, 4-stage pipeline stepper (*Applied $\to$ Shortlisted $\to$ Interview $\to$ Offered*), and timeline.
- **Shortlisting & Interview Panel:** Maker shortlisted applicant; panelist `checker@primebank.com.bd` scored and cleared Interview `HR-INT-2026-0003` (*Customer Service Panel*).
- **Job Offer & Acceptance:** Issued `HR-OFF-2026-00003`, submitted (`docstatus = 1`), and recorded candidate acceptance (`status = "Accepted"`).

### Module 04: Onboarding (Offer to First Morning)
- **Onboarding Template:** Verified standard template `HR-EMP-ONT-00001` with 4 onboarding activities (*IT Account Creation*, *ID Card Issuance*, *Desk Allocation*, *Induction Session*).
- **Zero-Re-Keying Handshake:** Created and submitted `Employee Onboarding` `HR-EMP-ONB-2026-00002` from accepted offer; auto-spawned boarding Project `PROJ-0005` and activity tasks.
- **Master Creation & Position Assignment:** Created Employee `HR-EMP-00023` (*Tanvir Ahmed*); assigned `POS-2026-00012` via `EPA-2026-00014`, flipping position status to `Filled`.
- **Checklist & Code of Conduct:** Collected 3 required documents (NID, Educational Certificate, Photos); recorded Code of Conduct delivery and acknowledgement (`2026-09-22`); onboarding marked `Completed`.
- **Employee Desk Profile:** Verified joiner profile reflects assigned position, designation, department, and branch.
- **Probation Confirmation:** Maker drafted Probation Evaluation `PE-2026-00007` (Score: 85%, Rating: 4/5, Recommendation: "Confirm"); Checker confirmed (`docstatus = 1`); stamped `custom_date_of_confirmation` (`2026-09-10`); verified `Confirmation Due Report`.

### Module 05: Employee Master Data & Privacy
- **Bilingual Master Record:** Created/verified `HR-EMP-00024` (*Nusrat Jahan* / *নুসরাত জাহান*); verified field-level security (`permlevel: 1`) protecting NID (`199512345678`), TIN (`123456789012`), e-TIN, and Birth Registration Number.
- **Child Tables:** Attached 2 family dependents (Spouse *Rahim Ahmed*, Son *Arif Ahmed* with 50% nominee shares each for benefits), 2 educational degrees (BBA, MBA), and regulated license (`JAIBB-2023-04521`).
- **Document Expiry Queue:** Tracked JAIBB Certificate expiring `2026-06-14`; verified `Employee Document Expiry Report` exception monitor in Desk.
- **Lawful Date of Birth Correction:** Corrected legacy entry typo (`1994-04-12` $\to$ `1995-04-12`) via `Employee Change Request` `ECR-2026-00013`. Maker applied `Check`, Checker approved (`Approved`, `applied = 1`); master DOB updated live with history intact.
- **Employee Self-Service Queue:** *Nusrat Jahan* logged into self-service, verified sensitive fields are hidden from view, and raised `HR Service Request` `HSR-2026-00003` for Data Correction; Maker initiated processing (`In Progress`), Checker completed with resolution note (`Completed`).
- **Auditor Verification:** Inspected 7 Version History entries on Employee master and verified immutable ECR before/after field changes.

---

## 3. Engineering Bugs Diagnosed & Resolved Today

During the testing cycles, I identified and resolved several critical runtime and configuration issues:

1. **Permission Hook False-Negative in `permission.py`:**
   - *Problem:* Returning `None` for privileged roles caused Frappe's `has_controller_permissions` hook to deny access to Desk forms.
   - *Fix:* Explicitly returned `True` for privileged roles (`HR Maker`, `HR Checker`, `HR User`, `System Manager`).
2. **User Permission Scoping Trap on Operational HR Users:**
   - *Problem:* Linking an Employee record to an HR User account caused Frappe to auto-apply an `allow: Employee` User Permission, blinding HR Maker and Checker from seeing other employees.
   - *Fix:* Removed the auto-scoping restriction for operational HR roles to maintain organization-wide view.
3. **Missing Holiday List on Separation:**
   - *Problem:* Submitting `Employee Separation` crashed because no Holiday List was assigned to company `UniSoft` covering the exit year.
   - *Fix:* Created `UniSoft Standard Holidays 2026` and submitted a `Holiday List Assignment`.
4. **Outgoing SMTP Email Crash in Offline / Local Dev:**
   - *Problem:* Completing an `HR Service Request` triggered `notify_on_completion()` which threw `OutgoingEmailError` when SMTP was unconfigured.
   - *Fix:* Wrapped `frappe.sendmail()` in safe exception handling in `apps/prime_bank_hrms/prime_bank_hrms/doctype/hr_service_request/hr_service_request.py`.
5. **Relief Reason Select Validation:**
   - *Problem:* Passing `"Internal Transfer"` to `relieve_assignment` was rejected by select field validation.
   - *Fix:* Standardized to allowed option `"Transfer"`.
6. **Built Automated QA Infrastructure:**
   - Created backend test automation modules: `setup_01.py`, `setup_02.py`, `setup_03.py`, `setup_04.py`, `setup_05.py`.
   - Created Playwright browser test runners: `run_story_01_test.js` through `run_story_05_test.js`.

---

## 4. Key Artifacts & Documentation Produced

1. **`test_10sept.md`:** Complete, consolidated technical test log containing the full evidence trails, tables, and record IDs for Stories 01–05.
2. **`test_10sept_issues_and_failures.md`:** Dedicated risk, issue tracking, and architectural vulnerability log for engineering reference.
3. **`sadat_10sept_report.md`:** This formal daily work submission report.

---

## 5. Live Records Created During Today's Testing

```text
Org Structure:           Dhaka Zone -> Dhaka Region -> Retail Banking -> Dhaka Main -> Retail Operations - Co.
Designation:             Customer Service Officer (Grade 3)
Positions:               POS-2026-00001 (Cards Operations Officer), POS-2026-00014 (Capacity: 2)
Position Request:        PR-2026-00003 (Approved, docstatus 1)
Position Assignments:    EPA-2026-00016 (Ended), EPA-2026-00014 (Active)
Employees Seeded:        HR-EMP-00001 (Tanvir Ahmed), HR-EMP-00009 (Farhana Yasmin), 
                         HR-EMP-00023 (Tanvir Ahmed - Joiner), HR-EMP-00024 (Nusrat Jahan)
User Accounts Provision: farhana.hire...@primebank.com.bd (Provisioned -> Disabled on exit)
Delegations:             DELEG-2026-00018 (Active), DELEG-2026-00019 (Expired)
Job Requisition:         HR-HIREQ-00006 (Open & Approved)
Job Opening:             HR-OPN-2026-0007 (Published with 2 screening questions)
Applicant & Token:       tanvir.candidate... (Token: GpXXsQWF46qhXcdW6XVPkw)
Interview & Offer:       HR-INT-2026-0003 (Cleared), HR-OFF-2026-00003 (Accepted)
Onboarding:              HR-EMP-ONB-2026-00002 (Completed), PROJ-0005
Probation Evaluation:    PE-2026-00007 (Confirmed, docstatus 1)
Change Requests (ECR):   ECR-2026-00010 (Approved), ECR-2026-00013 (Approved DOB 1995-04-12)
HR Service Request:      HSR-2026-00003 (Completed)
Regulated License:       JAIBB-2023-04521 (Expiry: 2026-06-14)
Audit Trail Records:     7 Version History Revisions verified on Employee Master
```

---

## 6. Next Steps & Recommendations

1. **Continue Coverage:** Proceed with automated browser testing for **Story 06 (Leave & Attendance)** and **Story 07 (Salary & Benefits)**.
2. **Permanent Hook for IAM User Permissions:** Formulate a Frappe hook preventing the default creation of single-employee user permissions for accounts holding `HR Maker`, `HR Checker`, or `HR User`.
3. **Docker Mailpit Container:** Add a lightweight SMTP testing sink (e.g. Mailpit) to Docker Compose to validate real email delivery on notifications without reliance on external relays.
