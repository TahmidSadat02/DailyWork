# Frontend Testing — prime_bank_hrm_erp

> Purpose: track manual frontend testing for this project — checklist, session notes, and bugs found.

## Setup

- App UI: `http://primebank.local:8080` (make sure `/etc/hosts` has `127.0.0.1 primebank.local`)
- Login: `administrator` + `ADMIN_PASSWORD` from `.env`
- Start the stack: `make up` (logs: `make logs`, status: `make ps`)
- After JS/CSS changes: `make build-assets`; after Python/DocType changes: `make sync`

## Test session info

| | |
|---|---|
| Date | 2026-09-09 |
| Tester | |
| Browser | |
| Stack status | paste `make ps` output here |

## Checklist

### 1. Login / Desk

- [ ] Login page loads
- [ ] Can log in as `administrator`
- [ ] Desk / home screen renders correctly
- [ ] English + Bangla labels display correctly (LN-01..03)

### 2. Custom app DocTypes (prime_bank_hr)

- [ ] Each custom DocType's list view opens
- [ ] New / Save / Submit (where applicable) works
- [ ] Maker–checker flow: same user cannot be both maker and checker (BR-19)
- [ ] Name fields have the English + Bangla pair

### 3. Separation / letters (`$P` print formats)

- [ ] experience_certificate
- [ ] no_dues_certificate
- [ ] relieving_letter
- [ ] resignation_acceptance_letter
- [ ] retirement_letter
- [ ] separation_approval_letter
- [ ] service_certificate

### 4. General UI

- [ ] No JS errors in the console (browser dev tools)
- [ ] Layout doesn't break on small screens
- [ ] Notifications / alerts display correctly

## Bugs found

| # | Date | Page / URL | Steps to reproduce | Expected | Actual | Screenshot |
|---|------|-----------|--------------------|----------|--------|-----------|
| 1 | | | | | | |

## Notes

- Screenshots can go in `gui-test-screenshots/`.
- Bug report summaries can also go in `qa_reports/`.
