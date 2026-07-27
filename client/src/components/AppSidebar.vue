<template>
  <aside class="sidebar" :class="{ collapsed: isCollapsed }">
    <div class="sidebar-header">
      <div class="logo-mark">{{ initials }}</div>
      <div v-if="!isCollapsed" class="logo-text">
        <h1>{{ t('nav.companyName') }}</h1>
        <span class="subtitle">{{ t('nav.subtitle') }}</span>
      </div>
    </div>

    <nav class="nav-list" aria-label="Main navigation">
      <router-link
        to="/"
        class="nav-item"
        :class="{ active: route.path === '/' }"
        :title="isCollapsed ? t('nav.overview') : null"
      >
        <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none" aria-hidden="true">
          <path d="M3 8.5L10 3L17 8.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M5 7.5V16C5 16.5523 5.44772 17 6 17H14C14.5523 17 15 16.5523 15 16V7.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M8 17V12H12V17" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        <span v-if="!isCollapsed" class="nav-label">{{ t('nav.overview') }}</span>
      </router-link>

      <router-link
        to="/inventory"
        class="nav-item"
        :class="{ active: route.path === '/inventory' }"
        :title="isCollapsed ? t('nav.inventory') : null"
      >
        <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none" aria-hidden="true">
          <path d="M10 3L17 6.5V13.5L10 17L3 13.5V6.5L10 3Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
          <path d="M3 6.5L10 10L17 6.5" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
          <path d="M10 10V17" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
        </svg>
        <span v-if="!isCollapsed" class="nav-label">{{ t('nav.inventory') }}</span>
      </router-link>

      <router-link
        to="/orders"
        class="nav-item"
        :class="{ active: route.path === '/orders' }"
        :title="isCollapsed ? t('nav.orders') : null"
      >
        <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none" aria-hidden="true">
          <path d="M6 3H14C14.5523 3 15 3.44772 15 4V17L10 14.5L5 17V4C5 3.44772 5.44772 3 6 3Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
          <path d="M7.5 7.5H12.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          <path d="M7.5 10.5H12.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        <span v-if="!isCollapsed" class="nav-label">{{ t('nav.orders') }}</span>
      </router-link>

      <router-link
        to="/spending"
        class="nav-item"
        :class="{ active: route.path === '/spending' }"
        :title="isCollapsed ? t('nav.finance') : null"
      >
        <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none" aria-hidden="true">
          <path d="M3 15.5L7.5 10.5L11 13L17 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M12.5 6H17V10.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        <span v-if="!isCollapsed" class="nav-label">{{ t('nav.finance') }}</span>
      </router-link>

      <router-link
        to="/demand"
        class="nav-item"
        :class="{ active: route.path === '/demand' }"
        :title="isCollapsed ? t('nav.demandForecast') : null"
      >
        <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none" aria-hidden="true">
          <path d="M3 17V3" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          <path d="M3 17H17" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          <path d="M5.5 14L9 9.5L12 12L16.5 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M13.5 6H16.5V9" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        <span v-if="!isCollapsed" class="nav-label">{{ t('nav.demandForecast') }}</span>
      </router-link>

      <router-link
        to="/reports"
        class="nav-item"
        :class="{ active: route.path === '/reports' }"
        :title="isCollapsed ? t('nav.reports') : null"
      >
        <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none" aria-hidden="true">
          <path d="M6 2.5H12L15 5.5V16C15 16.5523 14.5523 17 14 17H6C5.44772 17 5 16.5523 5 16V3.5C5 2.94772 5.44772 2.5 6 2.5Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
          <path d="M7.5 9H12.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          <path d="M7.5 12H12.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        <span v-if="!isCollapsed" class="nav-label">{{ t('nav.reports') }}</span>
      </router-link>
    </nav>

    <button
      class="collapse-toggle"
      @click="toggleCollapsed"
      :title="isCollapsed ? t('nav.expandSidebar') : t('nav.collapseSidebar')"
      :aria-label="isCollapsed ? t('nav.expandSidebar') : t('nav.collapseSidebar')"
      :aria-expanded="!isCollapsed"
    >
      <svg
        width="16"
        height="16"
        viewBox="0 0 16 16"
        fill="none"
        :class="{ flipped: isCollapsed }"
        aria-hidden="true"
      >
        <path d="M10 3L5 8L10 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
  </aside>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'
import { useI18n } from '../composables/useI18n'

const { t } = useI18n()
const route = useRoute()

const STORAGE_KEY = 'sidebar-collapsed'
// Restore collapsed state from localStorage so the layout persists across reloads
const isCollapsed = ref(localStorage.getItem(STORAGE_KEY) === 'true')

const initials = computed(() => {
  const name = t('nav.companyName') || ''
  return name
    .split(' ')
    .map(word => word[0])
    .filter(Boolean)
    .slice(0, 2)
    .join('')
    .toUpperCase()
})

const toggleCollapsed = () => {
  isCollapsed.value = !isCollapsed.value
  localStorage.setItem(STORAGE_KEY, String(isCollapsed.value))
}
</script>

<style scoped>
.sidebar {
  display: flex;
  flex-direction: column;
  width: 260px;
  height: 100vh;
  position: sticky;
  top: 0;
  flex-shrink: 0;
  background: #ffffff;
  border-right: 1px solid #e2e8f0;
  transition: width 0.2s ease;
  overflow: hidden;
}

.sidebar.collapsed {
  width: 72px;
}

.sidebar-header {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 20px 16px;
  border-bottom: 1px solid #e2e8f0;
  min-height: 72px;
}

.logo-mark {
  width: 36px;
  height: 36px;
  flex-shrink: 0;
  border-radius: 8px;
  background: linear-gradient(135deg, #2563eb 0%, #1e40af 100%);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 0.813rem;
  letter-spacing: 0.025em;
}

.logo-text {
  min-width: 0;
}

.logo-text h1 {
  font-size: 1rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.subtitle {
  display: block;
  font-size: 0.75rem;
  color: #64748b;
  font-weight: 400;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.nav-list {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 16px 12px;
  overflow-y: auto;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 12px;
  border-radius: 8px;
  color: #64748b;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.875rem;
  border-left: 3px solid transparent;
  transition: background-color 0.15s ease, color 0.15s ease;
  white-space: nowrap;
}

.sidebar.collapsed .nav-item {
  justify-content: center;
  padding: 10px 0;
}

.nav-icon {
  flex-shrink: 0;
}

.nav-label {
  overflow: hidden;
  text-overflow: ellipsis;
}

.nav-item:hover {
  color: #0f172a;
  background: #f1f5f9;
}

.nav-item.active {
  color: #2563eb;
  background: #eff6ff;
  border-left-color: #2563eb;
}

.collapse-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin: 12px;
  padding: 8px;
  background: none;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  color: #64748b;
  cursor: pointer;
  transition: all 0.2s ease;
}

.collapse-toggle:hover {
  background: #f8fafc;
  color: #0f172a;
  border-color: #cbd5e1;
}

.collapse-toggle svg {
  transition: transform 0.2s ease;
}

.collapse-toggle svg.flipped {
  transform: rotate(180deg);
}
</style>
