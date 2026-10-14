# QA Test Report: GTex ERP, Commercial & Buyer Orders

| Item | Detail |
|---|---|
| Tester | Sadat (Tahmid Sadat) |
| Test date | 12 Oct 2026 |
| Application | GTex ERP (garments factory ERP), area "Commercial & Buyer Orders" |
| Frontend / Backend | http://localhost:3000 (confirm port) / http://127.0.0.1:8000 (Frappe) |
| Test tool | Antigravity IDE, built-in browser agent |
| AI model | Gemini 3.8 Flash |
| Test guides used | `buyer_agents_m01_qa_guide.md` (M01), `order_book_merchandising_m02_m03_qa_guide.md` (M02, M03) |
| Environment | _fill in: dev / test database?_ |

---

## 1. Purpose

Check, in a live browser, that every screen in the Commercial & Buyer Orders area:

1. Loads correctly and every button is visible, in the right place, and goes to the right page.
2. Accepts data in every field and saves it.
3. Shows the saved record in the right place, and keeps it after a page reload.

## 2. Scope

| Group | Screens | Guide used | Tested today? |
|---|---|---|---|
| Overview | 3 | None (discovered from sidebar) | _fill in_ |
| Buyers & agents (M01) | 20 | M01 guide | _fill in_ |
| Order book (M02) | 12 | M02/M03 guide, Part 1 | _fill in_ |
| Merchandising (M03) | 14 | M02/M03 guide, Part 2 | _fill in_ |
| **Total** | **49** | | _fill in_ |

## 3. Method

The test ran in phases, one module at a time, so the AI agent would not lose track.

| Phase | What was done |
|---|---|
| 0 | Setup: login, screen list, rules file |
| A (per module) | Click and navigation check: buttons visible, correctly placed, going to the right page. No data entered |
| B (per module) | Fill every field with guide data (names prefixed `QA_AUTO_`), submit empty first to check validation, then save |
| C (per module) | Persistence check: record found in the right place, still there after reload, field values match |
| Final | Report generated from saved result files |

Rules followed: one click then one screenshot; no PASS without observation; linked records that do not exist are marked BLOCKED, not created; only `QA_AUTO_` records may be edited or deleted.

## 4. Summary of Results

_Fill in from the agent's result files (`test-reports/results/*.json`)._

| Metric | Count |
|---|---|
| Screens covered (of 49) | _ |
| PASS | _ |
| FAIL | _ |
| PLACEHOLDER (screen not built) | _ |
| READ-ONLY (save blocked) | _ |
| BLOCKED (missing linked record) | _ |
| NOT RUN | _ |
| Buttons checked | _ |
| Forms filled | _ |
| Records saved | _ |
| Records found in the right place | _ |
| Pass rate | _ % |

## 5. Result by Module

| Module | Screens | PASS | FAIL | PLACEHOLDER | BLOCKED | NOT RUN |
|---|---|---|---|---|---|---|
| Overview | 3 | _ | _ | _ | _ | _ |
| M01 Buyers & agents | 20 | _ | _ | _ | _ | _ |
| M02 Order book | 12 | _ | _ | _ | _ | _ |
| M03 Merchandising | 14 | _ | _ | _ | _ | _ |

## 6. Observations Seen

| Screen | Observation |
|---|---|
| Acquisition campaigns (M01) | Shows the banner "This screen is specified but not yet built in the prototype". Empty state with "Records 0 · view: Standard". New, Filter, Export, Search and Refresh are visible. Edit and Delete are greyed out (expected, no row selected). Footer shows "read replica", so saving may be blocked. |
| _add more_ | _ |

Notes:
- Screens showing the "not yet built" banner are marked PLACEHOLDER. This is not counted as an app bug.
- The "PASS" marks inside the guide files were written in advance. They are not test results.

## 7. Defect Log

| # | Route | Selector / Control | Category | Expected | Actual | Steps to reproduce | Screenshot |
|---|---|---|---|---|---|---|---|
| 1 | _ | _ | _ | _ | _ | _ | _ |

Categories: Button Missing, Button Misplaced, Wrong Navigation, New Does Nothing, Validation, Save Failed, Data Not Persisted, Wrong Save Location, Console Error, Network Error.

## 8. Save Verification

| Screen | Data entered | Where it should be saved (per guide) | Found there? (Y/N) | After reload? (Y/N) |
|---|---|---|---|---|
| _ | _ | _ | _ | _ |

## 9. Test Records Created (for cleanup)

All records created by this run start with `QA_AUTO_`.

| Screen | Record name | Deleted? |
|---|---|---|
| _ | _ | _ |

## 10. Limitations

- Console errors and network failures were checked only if the built-in browser could read them; otherwise marked NOT CHECKED.
- Guide-based checks of the database and compliance rules were done through the UI only, and marked "Not verifiable from UI" when not visible.
- AI agent results must be spot-checked by a human on a few screens.

## 11. Conclusion

_Fill in: overall status, main problems found, and next steps._

---

Prepared by: Sadat
