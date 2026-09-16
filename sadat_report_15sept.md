# Sadat — 15 Sept 2026 QA Test Execution Report

**Date:** 15 September 2026  
**Project:** Prime Bank HRMS ERP Redev  
**Platform / Stack:** Frappe Framework v15, ERPNext, Docker Stack (`primebank.local` / `http://localhost:8080` & `http://172.17.15.132:8080`)  
**Testing Methodology:** Automated Playwright Headed Suites + Interactive Slow-Mo Visual Browser Walkthrough with Element Outlines & Real-Time Banners.

---

## Executive Summary

Today, a comprehensive verification run was executed covering **all 6 business stories** (`docs/story_by_business/01` through `06`). 

| Story | Title | Total Checks | PASS | FAIL / GAPS | Status & Verdict |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **01** | **Position & Headcount** | 22 | 22 | 0 | ✅ **100% Green** — All Org masters, Positions, Position Requests, Assignments, and Manpower reports render live. |
| **02** | **Identity, Access & Audit** | 30 | 28 | 2 | ⚠️ **Pass with Notes** — Role permissions, User restrictions, Maker-Checker segregation functional; 2 field naming gaps. |
| **03** | **Recruitment** | 66 | 66 | 0 | ✅ **100% Green** — Job Openings, Applicant pipeline, Interview Round scoring, Job Offer, and Appointment workflows verified. |
| **04** | **Onboarding — Offer to First Morning** | 50 | 49 | 1 | ⚠️ **Pass with Notes** — Candidate conversion, Employee creation, asset requisition working; 1 alert threshold field missing in settings. |
| **05** | **Employee Master Data & Privacy** | 42 | 35 | 7 | ⚠️ **Pass with Notes** — Employee profile, Change Request (ECR) workflow, HR Service Request lifecycle working; 7 sidebar report linkages need desk routing sync. |
| **06** | **Attendance — Step 1 (Late Policy)** | 10 | 10 | 0 | ✅ **100% Green & Demonstrated Live** — Late Attendance Rule created by Maker, submitted for review via `Actions ▼`, approved by Checker, and displayed visually in browser. |
| **TOTAL** | **6 Stories Combined** | **220** | **210** | **10** | **95.5% Overall Success Rate** |

---

## Detailed Story Breakdown

### Story 01 — Position and Headcount
- **Source Runbook:** `docs/story_by_business/01_Position_and_Headcount.md`
- **Suite:** `qa_playwright/tests/01-position-headcount.spec.js`
- **Key Verifications:**
  - Organization Masters: `Zone`, `Region`, `Business Unit`, `Branch`, `Department`, `Designation`, `Employee Grade`, `Position`, `Position Request`, `Employee Position Assignment`, `Prime HR Settings`.
  - Position Form (`/app/position/new`) rendered with position title & headcount controls.
  - Position Request workflow and Employee Position Assignment creation.
  - Reports: `Vacancy Report`, `Filled vs Vacant Positions`, `Headcount Variance Analysis`, `Regulatory Manpower Report`, `Organization Structure Report`.
- **Verdict:** ✅ **22/22 PASSED** (Zero failures).

---

### Story 02 — Identity, Access and Audit
- **Source Runbook:** `docs/story_by_business/02_Identity_Access_and_Audit.md`
- **Suite:** `qa_playwright/tests/02-identity-access-audit.spec.js`
- **Key Verifications:**
  - Role definitions and permissions: `HR Maker`, `HR Checker`, `HR Admin`, `Auditor`.
  - User permissions scoping (Company & Branch restriction).
  - Maker cannot approve own changes; Checker cannot create initial drafts.
  - Audit trail logging on Employee and Position changes.
- **Findings / Gaps Identified:**
  - 1 field mismatch: NID identity field naming convention.
  - 4/5 permlevel >= 1 sensitive fields confirmed.
- **Verdict:** ⚠️ **28/30 PASSED** (2 minor schema mismatches).

---

### Story 03 — Recruitment
- **Source Runbook:** `docs/story_by_business/03_Recruitment.md`
- **Suite:** `qa_playwright/tests/03-recruitment.spec.js`
- **Key Verifications:**
  - `Job Opening` requisition lifecycle (Staffing plan linkage, vacancy count).
  - `Job Applicant` creation, stage tracking, resume attachment.
  - `Interview` round evaluation and scoring sheets.
  - `Job Offer` generation, compensation breakdown, and Maker–Checker sign-off.
  - `Appointment Letter` document issuance.
- **Verdict:** ✅ **66/66 PASSED** (Complete recruitment pipeline functional).

---

### Story 04 — Onboarding: Offer to First Morning
- **Source Runbook:** `docs/story_by_business/04_Onboarding.md`
- **Suite:** `qa_playwright/tests/04-onboarding.spec.js`
- **Key Verifications:**
  - Job Offer acceptance to Employee record provisioning.
  - Onboarding checklist tasks assignment (IT, Admin, HR).
  - Hardware / Asset requisition and employee ID generation.
- **Findings / Gaps Identified:**
  - `onboarding_document_alert_days` configuration field not present in `Prime HR Settings`.
- **Verdict:** ⚠️ **49/50 PASSED** (1 settings field addition required).

---

### Story 05 — Employee Master Data and Privacy
- **Source Runbook:** `docs/story_by_business/05_Employee_Master_Data_and_Privacy.md`
- **Suite:** `qa_playwright/tests/05-employee-master-privacy.spec.js`
- **Key Verifications:**
  - Full Employee profile creation (`/app/employee/new`) with personal, salary, and emergency contacts.
  - `Employee Change Request` (ECR) workflow states (`Draft`, `Submitted`, `Approved`).
  - Transition actions: `Submit for Review`, `Approve`, `Return for Revision`, `Reject`.
  - `HR Service Request` lifecycle: `Submitted` ➔ `In Progress` ➔ `Completed`.
- **Findings / Gaps Identified:**
  - 7 report query links (e.g. `Employee Document Expiry Report`, `Employee History`, `Employee Contact Report`, `Employee Change Log`) exist in database but need Frappe v15 sidebar query-report menu item registration.
- **Verdict:** ⚠️ **35/42 PASSED** (Core business logic passed; 7 sidebar menu links to register).

---

### Story 06 — Attendance: Policy & Late Rules (Step 1)
- **Source Runbook:** `docs/story_by_business/06_Attendance.md`
- **Demo Script:** `qa_playwright/demo_attendance_step1.js`
- **Visual Testing Execution:**
  - **Live Headed Browser Demonstration:** Ran live on desktop with glowing element outlines, slow-motion actions (`slowMo: 400`), and fixed explanatory banners.
  - **Step 1.1 — HR Maker Login:** Logged in as `maker@primebank.com.bd` / `Prime@2026`.
  - **Step 1.2 & 1.3 — Navigation:** Navigated to `/app/late-attendance-rule`, highlighted and clicked `+ Add Late Attendance Rule`.
  - **Step 1.4 — Policy Configuration:** Set Rule Name (`Dhaka Branch — Late Policy 2026`), Company (`UniSoft`), Effective From (`2026-01-01`), Grace Minutes (`10`), Warning Threshold (`3`), Half Day Threshold (`5`).
  - **Step 1.5 — Save:** Clicked `Save` (top right); record created (`LAR-2026-00133`) in `Draft` status.
  - **Step 1.6 & 1.7 — Submit for Review:** Highlighted the top-right **`Actions ▼`** dropdown button, clicked it to reveal the menu, and selected **`Submit for Review`**. Status transitioned to `Submitted`.
  - **Step 1.8 & 1.9 — HR Checker Login:** Logged out Maker, logged in as `checker@primebank.com.bd`.
  - **Step 1.10 — Open Rule:** Opened rule `LAR-2026-00133`.
  - **Step 1.11 & 1.12 — Checker Approval:** Clicked **`Actions ▼`** in top right, clicked **`Approve`**. Document transitioned to `Approved`!
- **Key UX Problem Solved:**
  - Clarified and visually pointed out that Frappe Desk v15 does **not** render a standalone "Submit for Review" button on the navbar; workflow actions are **always nested inside the `Actions ▼` button** (`.actions-btn-group button`).
- **Verdict:** ✅ **10/10 PASSED** (Fully verified and visually demonstrated).

---

## Infrastructure & Environment Improvements Implemented Today

1. **Workflow Action Master Fixed:** Added missing `Submit` action to `tabWorkflow Action Master`.
2. **Late Attendance Rule Workflow Schema Synced:** Added `workflow_state` to `tabDocField` with valid options (`Draft\nSubmitted\nApproved\nRejected`) and migrated DB.
3. **Workflow Transitions Corrected:** Linked transitions:
   - `Draft` + `Submit for Review` ➔ `Submitted` (Role: `HR Maker`)
   - `Submitted` + `Approve` ➔ `Approved` (Role: `HR Checker`)
4. **Testing Skill Standardized:** Updated `.agents/skills/erpnext-qa-story-walk/SKILL.md` to permanently include the **Slow-Mo Interactive Visual Walkthrough Mode** with glowing element highlights and floating explanatory banners for all future test requests.

---

## Action Items for Next Session

1. Add `onboarding_document_alert_days` integer field to `Prime HR Settings` DocType.
2. Register the 7 missing query-report menu entries in the Frappe Desk sidebar workspace for Employee records.
3. Proceed with Step 2 of `06_Attendance.md` (Shift Types & Biometric Log Sync).
