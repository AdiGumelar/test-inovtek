<script setup>
import { ref } from 'vue'
import Sidebar from '../components/Sidebar.vue'
import Navbar from '../components/Navbar.vue'

const isSidebarOpen = ref(false)

const openSidebar = () => {
  isSidebarOpen.value = true
}

const closeSidebar = () => {
  isSidebarOpen.value = false
}
</script>

<template>
  <div class="app-page dashboard-layout">
    <Sidebar :is-open="isSidebarOpen" @close="closeSidebar" />

    <div v-if="isSidebarOpen" class="sidebar-backdrop d-lg-none" @click="closeSidebar"></div>

    <div class="dashboard-main">
      <Navbar @toggle-sidebar="openSidebar" />

      <main class="content-area p-3 p-lg-4">
        <slot />
      </main>
    </div>
  </div>
</template>
