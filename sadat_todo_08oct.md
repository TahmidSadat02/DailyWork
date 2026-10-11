# Backend & Frontend Integration Plan — 08 Oct 2026
**Project:** Garments & Textile Industry ERP (GTex ERP)  
**Author:** Sadat  
**Date:** October 8, 2026  

---

## Primary Goal
Connect the Next.js frontend application with the Frappe/ERPNext custom backend modules (`grp_*`) via REST API and RPC endpoints, establishing dynamic data fetching, authentications, and fallback mechanisms while keeping all prototype UI interactions fully functional.

---

## Actionable Todo Items

### Phase 1: Environment & API Client Architecture Setup
- [ ] **1.1 Environment Configuration:** Setup `.env.local` in `frontend/` with `NEXT_PUBLIC_FRAPPE_URL` (e.g. `http://localhost:8006` or `http://gtex.localhost:8086`).
- [ ] **1.2 Frappe API Client (`lib/api/frappe-client.ts`):** Build a typed API wrapper handling:
  - Token Auth (`token API_KEY:API_SECRET`) & Cookie Session Auth (`sid`).
  - Standard DocType CRUD operations (`get_list`, `get_doc`, `create_doc`, `update_doc`).
  - Custom Whitelisted Method Calls (`frappe.call`).
  - Fallback logic to static `lib/data/demo.ts` when backend is offline or endpoint is empty.

---

### Phase 2: Authentication & User Context
- [ ] **2.1 Login Integration (`/login`):** Connect login form to `/api/method/login` endpoint with cookie management.
- [ ] **2.2 User Profile & Role Context:** Fetch logged-in user profile (`frappe.auth.get_logged_user`) and user roles to populate topbar user menu across all pages.
- [ ] **2.3 Session Persistence:** Add auth provider or middleware to protect `/ (erp)` routes and handle session timeout gracefully.

---

### Phase 3: Module-Wise API Integration

#### 3.1 Commercial & TNA Console (`/commercial/tna`)
- [ ] Wire TNA Critical Path data to DocType `TNA Order Plan` / `TNA Action Item`.
- [ ] Connect Stepper actions (Re-run, Chase List Generation, Re-plan simulation) to Frappe RPC methods.

#### 3.2 Supplier & Subcontractor Portal (`/vendor`)
- [ ] Connect Purchase Orders table to DocType `Purchase Order`.
- [ ] Connect RFQ & Sealed Bid submission modal to DocType `Supplier Quotation`.
- [ ] Connect Delivery Note upload to DocType `Purchase Receipt`.
- [ ] Connect Invoices to DocType `Purchase Invoice`.

#### 3.3 Buyer Portal (`/buyer`)
- [ ] Connect Buyer 360 orders & style status to DocType `Sales Order` & `Style Master`.
- [ ] Connect Techpack / Sample status tracking to live backend endpoints.

#### 3.4 Worker Self-Service (`/worker`)
- [ ] Connect Shift Attendance & Overtime logs to `Attendance`.
- [ ] Connect Leave Application form & modal to `Leave Application`.
- [ ] Connect Digital Payslip download to `Salary Slip`.

---

### Phase 4: UI Resilience & Error Handling
- [ ] **4.1 Fallback Provider:** Ensure seamless UI fallback to `demo.ts` mock data if API requests fail or backend is unreachable.
- [ ] **4.2 Loading States & Toast Notifications:** Add loading spinners/skeletons during API fetch and Sonner toasts (`toast.success`, `toast.error`) for write actions.
- [ ] **4.3 Type Safety:** Enforce strict Zod schemas / TypeScript interfaces on API payload returns.

---

### Phase 5: Verification & Quality Assurance
- [ ] **5.1 Typecheck:** Run `pnpm typecheck` in `frontend/` to confirm 0 TypeScript compilation errors.
- [ ] **5.2 End-to-End Test:** Test full flow from Login -> Dashboard -> TNA Console -> Portals with backend API connected.
