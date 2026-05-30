<script setup>
import { computed } from 'vue'
import MainLayout from '../layouts/MainLayout.vue'

const getDataFromLocalStorage = (key) => {
  const savedData = localStorage.getItem(key)

  if (!savedData) {
    return []
  }

  try {
    return JSON.parse(savedData)
  } catch (error) {
    return []
  }
}

const jobs = computed(() => getDataFromLocalStorage('recruitment_jobs'))
const candidates = computed(() => getDataFromLocalStorage('recruitment_candidates'))

const stats = computed(() => [
  {
    label: 'Total Jobs',
    value: jobs.value.length,
    icon: 'bi-briefcase',
    colorClass: 'text-primary',
    backgroundClass: 'bg-primary-subtle',
  },
  {
    label: 'Total Candidates',
    value: candidates.value.length,
    icon: 'bi-people',
    colorClass: 'text-success',
    backgroundClass: 'bg-success-subtle',
  },
  {
    label: 'Total Applications',
    value: candidates.value.length,
    icon: 'bi-file-earmark-text',
    colorClass: 'text-warning',
    backgroundClass: 'bg-warning-subtle',
  },
])
</script>

<template>
  <MainLayout>
    <div class="d-flex justify-content-between align-items-center mb-4">
      <div>
        <h1 class="h3 fw-semibold mb-1">Dashboard</h1>
        <p class="text-secondary mb-0">Ringkasan proses recruitment.</p>
      </div>
    </div>

    <div class="row g-3">
      <div v-for="item in stats" :key="item.label" class="col-md-4">
        <div class="card stat-card border-0 shadow-sm h-100">
          <div class="card-body p-4">
            <div class="d-flex align-items-start justify-content-between gap-3">
              <div>
                <p class="text-secondary mb-2">{{ item.label }}</p>
                <h2 class="display-6 fw-semibold mb-0">{{ item.value }}</h2>
              </div>

              <span
                class="stat-icon d-inline-flex align-items-center justify-content-center"
                :class="[item.colorClass, item.backgroundClass]"
              >
                <i class="bi fs-4" :class="item.icon"></i>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </MainLayout>
</template>
