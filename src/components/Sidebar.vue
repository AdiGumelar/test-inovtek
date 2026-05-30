<script setup>
import { RouterLink, useRoute, useRouter } from 'vue-router'

defineProps({
  isOpen: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['close'])

const route = useRoute()
const router = useRouter()

const menus = [
  { name: 'Dashboard', path: '/dashboard', icon: 'bi-speedometer2' },
  { name: 'Jobs', path: '/jobs', icon: 'bi-briefcase' },
  { name: 'Candidates', path: '/candidates', icon: 'bi-people' },
]

const isActiveMenu = (path) => route.path === path

const handleLogout = () => {
  localStorage.removeItem('recruitment_is_logged_in')
  localStorage.removeItem('recruitment_user_email')
  router.push('/')
}
</script>

<template>
  <aside class="sidebar bg-white border-end" :class="{ 'is-open': isOpen }">
    <div class="d-flex align-items-center justify-content-between px-4 py-3 border-bottom">
      <div class="d-flex align-items-center gap-2">
        <span class="brand-icon d-inline-flex align-items-center justify-content-center">
          <i class="bi bi-person-workspace"></i>
        </span>
        <span class="fw-semibold">Recruitment</span>
      </div>

      <button class="btn btn-light d-lg-none" type="button" @click="emit('close')">
        <i class="bi bi-x-lg"></i>
      </button>
    </div>

    <nav class="p-3">
      <RouterLink
        v-for="menu in menus"
        :key="menu.path"
        :to="menu.path"
        class="sidebar-link d-flex align-items-center gap-3 rounded-2 px-3 py-2 mb-2"
        :class="{ active: isActiveMenu(menu.path) }"
        @click="emit('close')"
      >
        <i class="bi" :class="menu.icon"></i>
        <span>{{ menu.name }}</span>
      </RouterLink>
    </nav>

    <div class="mt-auto p-3 border-top">
      <button class="btn btn-outline-danger w-100 text-start" type="button" @click="handleLogout">
        <i class="bi bi-box-arrow-left me-2"></i>
        Logout
      </button>
    </div>
  </aside>
</template>
