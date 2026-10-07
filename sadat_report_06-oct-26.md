# Development & Bug Fix Report · 06-Oct-2026

**Author / Engineer:** Sadat  
**Repository:** GTex ERP (`Textile`)  
**Branch:** `sadat`  
**Date:** 06 October 2026  
**Status:** All tasks tested, verified, and passing (`pnpm typecheck: 0 errors`)

---

## Executive Summary

During this work session, multiple critical UX, navigation, profile flyout, and prototype compliance issues were identified and resolved across the GTex Dynamics 365 ERP web application. All screens and behaviors now match the reference prototype (`prototype/`) 1:1, preserving code structure, clean modularity, and TypeScript type safety.

---

## 1. Issues Identified & Resolutions

### 1.1 My Settings (`/settings`) Section & Interactivity
- **Problem:** On `http://localhost:3000/settings`, only "Personal information" was active; selecting other tabs was unresponsive and features were non-functional. The "Saved views" section was also missing its standard Dynamics icon and actions.
- **Fix:**
  - Implemented state handling for all 8 Dynamics 365 settings tabs:
    1. **Personal information** (`set-personal`)
    2. **Role centre & company** (`set-context`) — functional dropdowns for role center, company entity, work date input with warning chip, and Save/Discard toast feedback.
    3. **Language & region** (`set-language`) — functional interface language, Bangla lakh-crore number format, and timezone selectors.
    4. **Appearance & density** (`set-appearance`) — interactive Comfortable (40px) vs Compact (30px) density toggle.
    5. **Notifications** (`set-notif`) — toggle switches for toasts, receipt posting, demand runs, approvals, and toast duration selector.
    6. **Saved views** (`set-views`) — added eye icon (`D365Icon name="eye"`) to the header and table; added interactive "Add view" button, view list, and "Publish" action toasts.
    7. **Personalization** (`set-personalize`) — export, import, and clear page actions.
    8. **Security** (`set-security`) — effective roles, visible modules, and session configuration display.

### 1.2 Home & Dashboard Navigation Links
- **Problem:** "Home" and "Dashboard" links were missing from the top navigation bar and sidebar sitemap.
- **Fix:**
  - **Top Bar (`top-bar.tsx`):** Added direct navbar links for Home (`/commercial`) and Dashboard (`/dashboard`) with `home` and `dashboard` icons alongside the mega-menu and company selector.
  - **Sidebar (`prototype-sidebar.tsx`):** Added "Home — dashboard" and "My Settings" to the Recent pinned section and verified sitemap catalogue integration.

### 1.3 Full Sidebar Sub-Module Interactivity (1,257 Screens)
- **Problem:** Clicking unbuilt sub-module screens in the sidebar did not open or navigate, leaving dead links across the 48 modules.
- **Fix:**
  - Connected `prototype-sidebar.tsx` to route all catalogue items to the Dynamics 365 standard view template at `/screen?p=area|group|screen`.
  - Added URL search parameter detection so the currently active screen is highlighted with the blue indicator pill and its parent accordion group automatically expands.

### 1.4 Profile Avatar Dropdown (`fly-user`) Bug Fix
- **Problem:** Clicking the user profile avatar (`NJ`) did not show or keep open the profile options; icons were missing or unstyled.
- **Fix:**
  - **Event Delegation Conflict:** In `prototype-behaviors.tsx`, global outside-click listeners were firing on `.tb-avatar` clicks and immediately stripping the `.open` class off `.tb-flyout`. Updated the handler to exempt topbar elements and elements with `data-flyout`.
  - **Event Propagation:** Updated `top-bar.tsx` so `toggleFlyout` stops event propagation (`e.stopPropagation()`) and assigned `data-flyout="fly-user"` to the avatar button.
  - **Missing Icon Styles:** Wrapped profile menu items (`My profile — My Settings`, `Buyer portal (view as)`, `Sign out`) in `<span className="ic">` matching `prototype/js/shell.js`.
  - **Default Icon Class:** Updated `d365-icons.tsx` so `D365Icon` always includes the `.ic` CSS class, ensuring SVG stroke, fill, and dimensions apply consistently.

### 1.5 Order Allocation & Size Breakup Standard View Screen (`screen/page.tsx`)
- **Problem:** Navigating to `http://localhost:3000/screen?p=commercial|Order book (M02)|Order allocation & size breakup` showed arbitrary mock data rows instead of the prototype's standard view anatomy from `prototype/screen.html`.
- **Fix:**
  - Re-implemented `frontend/app/(erp)/screen/page.tsx` to match `prototype/screen.html` verbatim:
    - **Breadcrumbs:** `Home` > `commercial` > `Order allocation & size breakup`.
    - **Header & Freshness:** `Order allocation & size breakup`, subtitle `Order book (M02) · standard view anatomy (floating command bar · message bar · grid · totals · all 8 states)`, and `as of 09:40 · read replica` badge.
    - **Command Bar:** `+ New` (primary), `Edit` (disabled), `Delete` (disabled), `Filter`, `Export`, `Search this view` input, and Refresh.
    - **Message Bar:** *"This screen is specified but not yet built in the prototype..."* with link to the module directory and dismiss button.
    - **Data Grid & Headers:** `RECORD`, `STATUS`, `AMOUNT`, `UPDATED`.
    - **Standard Honest Empty State:**
      - Header: *"Nothing here yet — and that is honest (standard empty state)"*
      - Subtext: *"When this screen goes live, every order allocation & size breakup record in the module appears here. Use New to create the first one, or remove filters."*
      - CTA: `+ Create the first record` button that opens the record creation modal.
    - **Grid Footer & Totals:** `Σ Totals` button and `Records 0 · view: Standard`.

---

## 2. File Change Summary

| File Path | Description of Changes |
| :--- | :--- |
| `frontend/app/(erp)/settings/page.tsx` | Implemented functional state for all 8 tabs; added Eye icon and dynamic "Add view" functionality to Saved views. |
| `frontend/app/(erp)/screen/page.tsx` | Aligned with `prototype/screen.html` standard view anatomy, empty state, table headers, and command bar. |
| `frontend/components/layout/top-bar.tsx` | Added Home & Dashboard navbar links; added `data-flyout` attributes and propagation handling; wrapped profile dropdown icons in `<span className="ic">`. |
| `frontend/components/layout/prototype-sidebar.tsx` | Integrated Home/Dashboard into Recent items; wired active sub-module highlighting and expansion via `useSearchParams`. |
| `frontend/components/behavior/prototype-behaviors.tsx` | Fixed outside-click delegation to exempt topbar buttons, eliminating flyout auto-close glitch. |
| `frontend/lib/d365-icons.tsx` | Added `eye` icon path; ensured `D365Icon` always renders with the `.ic` CSS class. |

---

## 3. Verification & Quality Assurance

1. **TypeScript Compilation:**
   ```bash
   pnpm typecheck
   # Output: $ tsc --noEmit (Exit Code: 0, 0 errors)
   ```
2. **Browser Testing (Playwright / Chromium Subagent):**
   - Verified `/settings` tab switching, density toggle, and view addition.
   - Verified topbar profile menu toggling upon clicking `NJ`, rendering `id`, `globe`, and `logout` icons properly.
   - Verified navigation from profile dropdown to `/settings`.
   - Verified `/screen?p=commercial|Order book (M02)|Order allocation & size breakup` renders the exact standard view anatomy matching the reference prototype.
