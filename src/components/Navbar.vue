<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const emit = defineEmits(['toggle-sidebar'])

const route = useRoute()

const pageTitle = computed(() => {
  const titles = {
    dashboard: 'Dashboard',
    jobs: 'Jobs',
    candidates: 'Candidates',
  }

  return titles[route.name] || 'Recruitment'
})

const userEmail = localStorage.getItem('recruitment_user_email') || 'admin@recruitment.test'
</script>

<template>
  <header class="top-navbar bg-white border-bottom px-3 px-lg-4">
    <div class="d-flex align-items-center justify-content-between h-100">
      <div class="d-flex align-items-center gap-3">
        <button class="btn btn-light d-lg-none" type="button" @click="emit('toggle-sidebar')">
          <i class="bi bi-list fs-5"></i>
        </button>

        <div>
          <h1 class="h5 fw-semibold mb-0">{{ pageTitle }}</h1>
          <small class="text-secondary">Mini Recruitment Dashboard</small>
        </div>
      </div>

      <div class="d-flex align-items-center gap-2">
        <span class="user-avatar d-inline-flex align-items-center justify-content-center">
          <i class="bi bi-person"></i>
        </span>
        <div class="d-none d-sm-block">
          <p class="small fw-medium mb-0">Recruiter</p>
          <p class="small text-secondary mb-0">{{ userEmail }}</p>
        </div>
      </div>
    </div>
  </header>
</template>
