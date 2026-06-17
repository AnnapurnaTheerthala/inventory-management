---
name: vue-saas-redesign
description: Redesign a Vue 3 app's UI from a horizontal top navigation bar to a modern SaaS-style vertical sidebar layout. Transforms App.vue to a fixed 240px sidebar with logo, nav items with inline SVG icons, user controls at bottom, and moves FilterBar inline into the main content area. Preserves all composables, view files, api.js, and global utility CSS classes.
---

# Vue SaaS Sidebar Redesign

This skill redesigns `client/src/App.vue` from a horizontal top-navigation layout to a modern SaaS-style fixed vertical sidebar. All composables, view components, and global utility CSS classes are preserved unchanged.

## Scope Boundary

**Touch only:**
- `client/src/App.vue` — template structure + layout CSS
- `client/src/components/FilterBar.vue` — one CSS value only (`top: 70px` → `top: 0`)

**Do NOT touch:**
- `client/src/composables/useFilters.js`
- `client/src/composables/useAuth.js`
- `client/src/composables/useI18n.js`
- `client/src/views/*.vue` (Dashboard, Inventory, Orders, Spending, Demand, Reports)
- `client/src/api.js`
- `client/src/main.js`
- `client/src/components/ProfileMenu.vue`
- `client/src/components/LanguageSwitcher.vue`

---

## Phase 1: Discovery Checklist

Before writing any code, read `client/src/App.vue` in full. Confirm:

- [ ] Template root is `<div class="app">` containing `<header class="top-nav">`, `<FilterBar />`, and `<main class="main-content">`
- [ ] Six `<router-link>` elements with paths: `/`, `/inventory`, `/orders`, `/spending`, `/demand`, `/reports`
- [ ] Nav labels use i18n keys: `t('nav.overview')`, `t('nav.inventory')`, `t('nav.orders')`, `t('nav.finance')`, `t('nav.demandForecast')`, and hard-coded `"Reports"`
- [ ] `<LanguageSwitcher />` and `<ProfileMenu @show-profile-details @show-tasks>` are in the header
- [ ] Global `<style>` (non-scoped) defines: resets, `.app`, `.top-nav`, `.nav-container`, `.nav-tabs`, `.logo`, `.subtitle`, `.main-content`, `.page-header`, `.stats-grid`, `.stat-card`, `.card`, `.card-header`, `.card-title`, `.table-container`, `table`/`thead`/`th`/`td`/`tbody`, `.badge.*`, `.loading`, `.error`
- [ ] `<script>` uses `export default` with `setup()` returning `{ t, showProfileDetails, showTasks, tasks, addTask, deleteTask, toggleTask }`
- [ ] `FilterBar.vue` has `position: sticky; top: 70px` in its scoped styles

If any of the above differs, adapt the templates below accordingly.

---

## Phase 2: New App.vue Template

Replace the `<template>` block with the following. **The `<script>` block is unchanged — copy it verbatim.**

```vue
<template>
  <div class="app">
    <!-- SIDEBAR -->
    <aside class="sidebar">
      <!-- Brand area -->
      <div class="sidebar-brand">
        <div class="brand-icon">
          <svg width="22" height="22" viewBox="0 0 22 22" fill="none">
            <rect x="2" y="2" width="8" height="8" rx="2" fill="#2563eb"/>
            <rect x="12" y="2" width="8" height="8" rx="2" fill="#93c5fd"/>
            <rect x="2" y="12" width="8" height="8" rx="2" fill="#93c5fd"/>
            <rect x="12" y="12" width="8" height="8" rx="2" fill="#2563eb"/>
          </svg>
        </div>
        <div class="brand-text">
          <span class="brand-name">{{ t('nav.companyName') }}</span>
          <span class="brand-subtitle">{{ t('nav.subtitle') }}</span>
        </div>
      </div>

      <!-- Primary navigation -->
      <nav class="sidebar-nav">
        <router-link to="/" class="nav-item" :class="{ active: $route.path === '/' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="2" y="2" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="2" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="2" y="10" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="10" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span>{{ t('nav.overview') }}</span>
        </router-link>

        <router-link to="/inventory" class="nav-item" :class="{ active: $route.path === '/inventory' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="2" y="2" width="14" height="14" rx="2" stroke="currentColor" stroke-width="1.5"/>
            <path d="M5 6h8M5 9h8M5 12h5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span>{{ t('nav.inventory') }}</span>
        </router-link>

        <router-link to="/orders" class="nav-item" :class="{ active: $route.path === '/orders' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M2 5h14l-1.5 8H3.5L2 5z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <circle cx="6" cy="15.5" r="1" fill="currentColor"/>
            <circle cx="12" cy="15.5" r="1" fill="currentColor"/>
          </svg>
          <span>{{ t('nav.orders') }}</span>
        </router-link>

        <router-link to="/spending" class="nav-item" :class="{ active: $route.path === '/spending' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <circle cx="9" cy="9" r="7" stroke="currentColor" stroke-width="1.5"/>
            <path d="M9 4.5v1M9 12.5v1M6.5 10c0 .83.67 1.5 1.5 1.5h2c.83 0 1.5-.67 1.5-1.5S10.83 8.5 10 8.5H8c-.83 0-1.5-.67-1.5-1.5S7.17 5.5 8 5.5h2c.83 0 1.5.67 1.5 1.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span>{{ t('nav.finance') }}</span>
        </router-link>

        <router-link to="/demand" class="nav-item" :class="{ active: $route.path === '/demand' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M2 14l4-4 3 2 4-5 3 3" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M2 16h14" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span>{{ t('nav.demandForecast') }}</span>
        </router-link>

        <router-link to="/reports" class="nav-item" :class="{ active: $route.path === '/reports' }">
          <svg class="nav-icon" width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="3" y="2" width="12" height="14" rx="2" stroke="currentColor" stroke-width="1.5"/>
            <path d="M6 6h6M6 9h6M6 12h4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span>Reports</span>
        </router-link>
      </nav>

      <!-- Footer: language switcher + profile menu -->
      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <!-- MAIN AREA -->
    <div class="main-wrapper">
      <FilterBar />
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <!-- MODALS -->
    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>
```

---

## Phase 3: Replacement CSS

Replace the entire `<style>` block (global, non-scoped). The layout rules change; all utility classes are preserved verbatim.

```css
/* ================================================
   RESETS + TYPOGRAPHY
   ================================================ */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f8fafc;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ================================================
   ROOT LAYOUT
   ================================================ */
.app {
  display: flex;
  min-height: 100vh;
}

/* ================================================
   SIDEBAR
   ================================================ */
.sidebar {
  width: 240px;
  min-width: 240px;
  background: #ffffff;
  border-right: 1px solid #e2e8f0;
  box-shadow: 1px 0 8px 0 rgba(0, 0, 0, 0.04);
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  z-index: 100;
}

/* Brand */
.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1.25rem 1rem 1rem;
  border-bottom: 1px solid #f1f5f9;
  flex-shrink: 0;
}

.brand-icon {
  flex-shrink: 0;
  width: 36px;
  height: 36px;
  background: #eff6ff;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.brand-text {
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.brand-name {
  font-size: 0.875rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.015em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.brand-subtitle {
  font-size: 0.688rem;
  color: #94a3b8;
  font-weight: 400;
  margin-top: 1px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Navigation */
.sidebar-nav {
  flex: 1;
  padding: 0.75rem 0.625rem;
  display: flex;
  flex-direction: column;
  gap: 0.125rem;
  overflow-y: auto;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.75rem;
  border-radius: 8px;
  color: #64748b;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: background 0.15s ease, color 0.15s ease;
  position: relative;
}

.nav-item:hover {
  background: #f1f5f9;
  color: #0f172a;
}

.nav-item.active {
  background: #eff6ff;
  color: #2563eb;
}

.nav-item.active::before {
  content: '';
  position: absolute;
  left: -0.625rem;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 60%;
  background: #2563eb;
  border-radius: 0 3px 3px 0;
}

.nav-icon {
  flex-shrink: 0;
  opacity: 0.85;
}

.nav-item.active .nav-icon {
  opacity: 1;
}

/* Footer */
.sidebar-footer {
  padding: 0.75rem 0.75rem 1rem;
  border-top: 1px solid #f1f5f9;
  display: flex;
  flex-direction: column;
  gap: 0.375rem;
  flex-shrink: 0;
}

/* ================================================
   MAIN WRAPPER — offset by sidebar width
   ================================================ */
.main-wrapper {
  margin-left: 240px;
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  min-width: 0;
}

/* ================================================
   MAIN CONTENT
   ================================================ */
.main-content {
  flex: 1;
  padding: 1.5rem 2rem;
  max-width: 1360px;
  width: 100%;
}

/* ================================================
   PAGE STRUCTURE
   ================================================ */
.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: #64748b;
  font-size: 0.938rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

/* ================================================
   UTILITY CLASSES — PRESERVED VERBATIM
   ================================================ */
.stat-card {
  background: white;
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: #64748b;
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value { color: #ea580c; }
.stat-card.success .stat-value { color: #059669; }
.stat-card.danger .stat-value  { color: #dc2626; }
.stat-card.info .stat-value    { color: #2563eb; }

.card {
  background: white;
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success    { background: #d1fae5; color: #065f46; }
.badge.warning    { background: #fed7aa; color: #92400e; }
.badge.danger     { background: #fecaca; color: #991b1b; }
.badge.info       { background: #dbeafe; color: #1e40af; }
.badge.increasing { background: #d1fae5; color: #065f46; }
.badge.decreasing { background: #fecaca; color: #991b1b; }
.badge.stable     { background: #e0e7ff; color: #3730a3; }
.badge.high       { background: #fecaca; color: #991b1b; }
.badge.medium     { background: #fed7aa; color: #92400e; }
.badge.low        { background: #dbeafe; color: #1e40af; }

.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}
```

---

## Phase 4: FilterBar Adjustment

`FilterBar` moves from between `<header>` and `<main>` to inside `.main-wrapper` above `.main-content`. Its sticky offset must change because there is no top nav above it anymore.

In `client/src/components/FilterBar.vue`, find the scoped `<style>` rule for `.filters-bar` and change:

```css
/* Before */
top: 70px;

/* After */
top: 0;
```

Everything else in `FilterBar.vue` is unchanged.

---

## Phase 5: CSS Classes to Remove

These classes exist in the original global `<style>` and must **not** appear in the new version:

| Removed class | Replaced by |
|---|---|
| `.top-nav` | `.sidebar` |
| `.nav-container` | `.sidebar-brand` + `.sidebar-nav` |
| `.nav-tabs` | `.sidebar-nav` |
| `.nav-tabs a` / `:hover` / `.active` / `.active::after` | `.nav-item` variants |
| `.logo` | `.sidebar-brand` |
| `.logo h1` | `.brand-name` |
| `.subtitle` | `.brand-subtitle` |
| `.nav-container > .nav-tabs` | — (layout handled differently) |
| `.nav-container > .language-switcher` | — |
| Old `.app { flex-direction: column }` | `.app { display: flex }` (row) |
| Old `.main-content { max-width: 1600px; margin: 0 auto }` | New `.main-content { max-width: 1360px }` |

---

## Phase 6: Design Tokens Reference

| Property | Value | Reason |
|---|---|---|
| Sidebar width | 240px | Fits all nav labels; standard SaaS breakpoint |
| Sidebar background | `#ffffff` | Clean; matches card aesthetic |
| Sidebar border | `1px solid #e2e8f0` | Matches existing design tokens |
| Nav item hover bg | `#f1f5f9` | Matches original `.nav-tabs a:hover` |
| Nav item active bg | `#eff6ff` | Matches original `.nav-tabs a.active` |
| Nav item active text | `#2563eb` | Primary blue, unchanged |
| Active indicator | 3px left bar, `#2563eb` | Vertical equiv of original bottom border |
| Nav icons | Inline SVG 18×18 `stroke="currentColor"` | No library dependency |
| Main wrapper offset | `margin-left: 240px` | Matches sidebar width exactly |
| FilterBar sticky top | `0` (was `70px`) | No top nav to clear |
| Main content max-width | `1360px` (was `1600px`) | Accounts for 240px sidebar |

---

## Phase 7: Migration Steps

Execute in this order:

1. Read `client/src/App.vue` in full — confirm current state matches the discovery checklist
2. Replace the `<template>` block with the new sidebar template from Phase 2
3. Leave the `<script>` block completely unchanged
4. Replace the entire `<style>` block with the CSS from Phase 3
5. Open `client/src/components/FilterBar.vue` — change `top: 70px` → `top: 0` in its scoped styles
6. Run the dev server and verify visually (see Phase 8)

---

## Phase 8: Verification

Start the dev server:

```bash
# Backend (port 8001)
cd server && uv run python main.py &

# Frontend (port 3000)
cd client && npm run dev
```

Visual checks at `http://localhost:3000`:

- [ ] Sidebar visible on left, 240px wide, white background, subtle right shadow
- [ ] Brand icon (2×2 blue grid) and company name/subtitle appear in sidebar header
- [ ] All 6 nav items listed vertically with icons
- [ ] Clicking each nav item routes correctly; active item shows blue text + left accent bar
- [ ] Hovering non-active items shows gray background
- [ ] FilterBar (4 dropdowns) appears as sticky bar at top of right content area
- [ ] FilterBar does not overlap the sidebar
- [ ] Main content scrolls independently of the sidebar
- [ ] LanguageSwitcher and ProfileMenu appear in sidebar footer
- [ ] Language switching updates nav item labels
- [ ] ProfileMenu opens; Profile Details and My Tasks modals render correctly
- [ ] All view pages render `.card`, `.stat-card`, `.badge`, `.table-container` correctly
- [ ] No horizontal scrollbar at 1440px viewport width
- [ ] Layout stable at 1280px minimum viewport

---

## Common Pitfalls

**ProfileMenu/LanguageSwitcher dropdown clipping:**
If dropdowns are cut off inside the sidebar, the cause is the sidebar's scroll container. Fix: add `overflow: visible` to `.sidebar` and instead scope the scrolling to `.sidebar-nav` alone (which already has `overflow-y: auto; flex: 1`).

**ProfileMenu dropdown opens to the wrong side:**
The dropdown is anchored `right: 0` of its button. In the sidebar footer, this may push it off-screen left. If so, add a CSS override in App.vue's global `<style>`:

```css
.sidebar-footer .dropdown-menu {
  left: 0;
  right: auto;
}
```

**FilterBar still offset:**
If FilterBar is partially hidden, verify `top: 70px` was changed to `top: 0` in `FilterBar.vue`'s scoped styles.

**Content feels narrow:**
If views look too narrow, increase `max-width` on `.main-content` or remove it to allow full width within the right panel.
