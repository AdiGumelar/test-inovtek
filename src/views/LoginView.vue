<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const email = ref('')
const password = ref('')
const errorMessage = ref('')

const handleLogin = () => {
  const isValidEmail = email.value === 'admin@recruit.com'
  const isValidPassword = password.value === '123456'

  if (isValidEmail && isValidPassword) {
    localStorage.setItem('recruitment_is_logged_in', 'true')
    localStorage.setItem('recruitment_user_email', email.value)

    router.push('/dashboard')
    return
  }

  errorMessage.value = 'Email atau password salah. Silakan coba lagi.'
}
</script>

<template>
  <div class="login-page app-page d-flex align-items-center justify-content-center px-3 py-4">
    <div class="card login-card border-0 shadow-sm">
      <div class="card-body p-4 p-md-5">
        <div class="text-center mb-4">
          <span class="brand-icon d-inline-flex align-items-center justify-content-center mb-3">
            <i class="bi bi-person-workspace"></i>
          </span>
          <h1 class="h4 fw-semibold mb-2">Login</h1>
          <p class="text-secondary mb-0">Masuk ke Mini Recruitment Dashboard.</p>
        </div>

        <div v-if="errorMessage" class="alert alert-danger" role="alert">
          <i class="bi bi-exclamation-circle me-2"></i>
          {{ errorMessage }}
        </div>

        <form @submit.prevent="handleLogin">
          <div class="mb-3">
            <label for="email" class="form-label">Email</label>
            <input
              id="email"
              v-model="email"
              type="email"
              class="form-control"
              placeholder="admin@recruit.com"
              required
            />
          </div>

          <div class="mb-4">
            <label for="password" class="form-label">Password</label>
            <input
              id="password"
              v-model="password"
              type="password"
              class="form-control"
              placeholder="Masukkan password"
              required
            />
          </div>

          <button type="submit" class="btn btn-primary w-100">
            <i class="bi bi-box-arrow-in-right me-2"></i>
            Login
          </button>
        </form>

        <div class="login-hint mt-4 p-3 rounded-2">
          <p class="small fw-semibold mb-1">Akun dummy</p>
          <p class="small text-secondary mb-0">Email: admin@recruit.com</p>
          <p class="small text-secondary mb-0">Password: 123456</p>
        </div>
      </div>
    </div>
  </div>
</template>
