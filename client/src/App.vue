<template>
  <div class="app">
    <!-- SIDEBAR -->
    <aside class="sidebar">
      <div class="sidebar-brand">
        <div class="brand-icon">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <rect x="1" y="1" width="8" height="8" rx="2" fill="#60a5fa"/>
            <rect x="11" y="1" width="8" height="8" rx="2" fill="#3b82f6"/>
            <rect x="1" y="11" width="8" height="8" rx="2" fill="#3b82f6"/>
            <rect x="11" y="11" width="8" height="8" rx="2" fill="#60a5fa"/>
          </svg>
        </div>
        <div class="brand-text">
          <span class="brand-name">{{ t('nav.companyName') }}</span>
          <span class="brand-subtitle">{{ t('nav.subtitle') }}</span>
        </div>
      </div>

      <div class="sidebar-body">
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

        <!-- Collapsible Filters -->
        <div class="sidebar-filters">
          <button class="filters-toggle" @click="filtersExpanded = !filtersExpanded">
            <svg width="15" height="15" viewBox="0 0 15 15" fill="none">
              <path d="M1.5 3.5h12M3.5 7.5h8M5.5 11.5h4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
            <span>Filters</span>
            <svg
              class="chevron-icon"
              :class="{ 'chevron-open': filtersExpanded }"
              width="14" height="14" viewBox="0 0 14 14" fill="none"
            >
              <path d="M3 5l4 4 4-4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </button>
          <div v-show="filtersExpanded" class="filters-panel">
            <FilterBar />
          </div>
        </div>
      </div>

      <!-- Footer: language + profile -->
      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <!-- MAIN -->
    <div class="main-wrapper">
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

<script>
import { ref, onMounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const filtersExpanded = ref(false)
    const apiTasks = ref([])

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(loadTasks)

    return {
      t,
      showProfileDetails,
      showTasks,
      filtersExpanded,
      tasks,
      addTask,
      deleteTask,
      toggleTask
    }
  }
}
</script>

<style>
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
  background: #f1f5f9;
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
  width: 248px;
  min-width: 248px;
  background: #1e293b;
  border-right: 1px solid #0f172a;
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
  padding: 1.25rem 1rem;
  border-bottom: 1px solid #334155;
  flex-shrink: 0;
}

.brand-icon {
  flex-shrink: 0;
  width: 34px;
  height: 34px;
  background: rgba(96, 165, 250, 0.15);
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
  color: #f1f5f9;
  letter-spacing: -0.01em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.brand-subtitle {
  font-size: 0.688rem;
  color: #475569;
  font-weight: 400;
  margin-top: 2px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Scrollable body (nav + filters) */
.sidebar-body {
  flex: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  scrollbar-width: thin;
  scrollbar-color: #334155 transparent;
}

.sidebar-body::-webkit-scrollbar {
  width: 4px;
}

.sidebar-body::-webkit-scrollbar-track {
  background: transparent;
}

.sidebar-body::-webkit-scrollbar-thumb {
  background: #334155;
  border-radius: 4px;
}

/* Navigation */
.sidebar-nav {
  padding: 0.75rem 0.625rem 0.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.125rem;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.75rem;
  border-radius: 8px;
  color: #94a3b8;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: background 0.15s ease, color 0.15s ease;
  position: relative;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #e2e8f0;
}

.nav-item.active {
  background: rgba(96, 165, 250, 0.12);
  color: #60a5fa;
}

.nav-item.active::before {
  content: '';
  position: absolute;
  left: -0.625rem;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 55%;
  background: #60a5fa;
  border-radius: 0 3px 3px 0;
}

.nav-icon {
  flex-shrink: 0;
  opacity: 0.8;
}

.nav-item.active .nav-icon {
  opacity: 1;
}

/* Collapsible filters section */
.sidebar-filters {
  margin: 0.5rem 0.625rem 0.75rem;
  border-top: 1px solid #2d3f55;
  padding-top: 0.625rem;
}

.filters-toggle {
  display: flex;
  align-items: center;
  gap: 0.625rem;
  width: 100%;
  padding: 0.5rem 0.75rem;
  background: none;
  border: none;
  border-radius: 8px;
  color: #64748b;
  font-size: 0.813rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.15s ease, color 0.15s ease;
  text-align: left;
}

.filters-toggle:hover {
  background: rgba(255, 255, 255, 0.05);
  color: #94a3b8;
}

.filters-toggle span {
  flex: 1;
}

.chevron-icon {
  transition: transform 0.2s ease;
  flex-shrink: 0;
}

.chevron-open {
  transform: rotate(180deg);
}

.filters-panel {
  margin-top: 0.375rem;
}

/* ================================================
   FILTERBAR OVERRIDES FOR SIDEBAR CONTEXT
   These override FilterBar.vue's scoped styles when
   the component is rendered inside the dark sidebar.
   ================================================ */
.sidebar-filters .filters-bar {
  background: transparent !important;
  border-bottom: none !important;
  padding: 0 !important;
  position: static !important;
  z-index: auto !important;
}

.sidebar-filters .filters-container {
  max-width: none !important;
  margin: 0 !important;
  padding: 0 0.75rem !important;
  flex-direction: column !important;
  gap: 0.5rem !important;
  align-items: stretch !important;
}

.sidebar-filters .filters-grid {
  flex-direction: column !important;
  gap: 0.5rem !important;
  align-items: stretch !important;
}

.sidebar-filters .filter-group {
  flex-direction: column !important;
  align-items: flex-start !important;
  gap: 0.25rem !important;
}

.sidebar-filters .filter-group label {
  font-size: 0.688rem !important;
  font-weight: 600 !important;
  color: #475569 !important;
  text-transform: uppercase !important;
  letter-spacing: 0.04em !important;
}

.sidebar-filters .filter-select {
  width: 100% !important;
  min-width: 0 !important;
  background: #0f172a !important;
  border: 1px solid #334155 !important;
  color: #cbd5e1 !important;
  font-size: 0.75rem !important;
  padding: 0.375rem 0.5rem !important;
  border-radius: 6px !important;
}

.sidebar-filters .filter-select:hover {
  border-color: #475569 !important;
}

.sidebar-filters .filter-select:focus {
  outline: none !important;
  border-color: #60a5fa !important;
  box-shadow: 0 0 0 2px rgba(96, 165, 250, 0.2) !important;
}

.sidebar-filters .reset-filters-btn {
  background: #0f172a !important;
  border: 1px solid #334155 !important;
  color: #64748b !important;
  width: 100% !important;
  padding: 0.375rem 0.5rem !important;
  border-radius: 6px !important;
  font-size: 0.75rem !important;
  gap: 0.375rem !important;
  margin-top: 0.25rem !important;
}

.sidebar-filters .reset-filters-btn:hover:not(:disabled) {
  background: rgba(96, 165, 250, 0.08) !important;
  border-color: #60a5fa !important;
  color: #93c5fd !important;
}

.sidebar-filters .reset-filters-btn svg {
  width: 14px !important;
  height: 14px !important;
}

/* Sidebar footer */
.sidebar-footer {
  padding: 0.75rem;
  border-top: 1px solid #334155;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  flex-shrink: 0;
}

/* ================================================
   MAIN WRAPPER
   ================================================ */
.main-wrapper {
  margin-left: 248px;
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
  padding: 1.75rem 2rem;
  max-width: 1400px;
  width: 100%;
}

/* ================================================
   PAGE STRUCTURE
   ================================================ */
.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.75rem;
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
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

/* ================================================
   UTILITY CLASSES — PRESERVED
   ================================================ */
.stat-card {
  background: white;
  padding: 1.375rem 1.5rem;
  border-radius: 12px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.07);
  transform: translateY(-1px);
}

.stat-label {
  color: #64748b;
  font-size: 0.813rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.6px;
  margin-bottom: 0.75rem;
}

.stat-value {
  font-size: 2.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.03em;
}

.stat-card.warning .stat-value { color: #ea580c; }
.stat-card.success .stat-value { color: #059669; }
.stat-card.danger .stat-value  { color: #dc2626; }
.stat-card.info .stat-value    { color: #2563eb; }

.card {
  background: white;
  border-radius: 12px;
  padding: 1.375rem 1.5rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.25rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #f1f5f9;
}

.card-title {
  font-size: 1rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.015em;
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
  border-top: 1px solid #f1f5f9;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.625rem 0.875rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.625rem 0.875rem;
  border-top: 1px solid #f8fafc;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.12s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

.badge {
  display: inline-block;
  padding: 0.25rem 0.625rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success    { background: #dcfce7; color: #166534; }
.badge.warning    { background: #fed7aa; color: #92400e; }
.badge.danger     { background: #fee2e2; color: #991b1b; }
.badge.info       { background: #dbeafe; color: #1e40af; }
.badge.increasing { background: #dcfce7; color: #166534; }
.badge.decreasing { background: #fee2e2; color: #991b1b; }
.badge.stable     { background: #e0e7ff; color: #3730a3; }
.badge.high       { background: #fee2e2; color: #991b1b; }
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
  border-radius: 10px;
  margin: 1rem 0;
  font-size: 0.875rem;
}
</style>
