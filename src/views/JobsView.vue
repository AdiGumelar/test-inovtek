<script setup>
import { computed, onMounted, ref } from "vue";
import MainLayout from "../layouts/MainLayout.vue";

const storageKey = "recruitment_jobs";

const dummyJobs = [
  {
    id: 1,
    title: "Frontend Developer",
    department: "Engineering",
    location: "Jakarta",
    status: "Open",
  },
  {
    id: 2,
    title: "HR Generalist",
    department: "People",
    location: "Bandung",
    status: "Closed",
  },
  {
    id: 3,
    title: "UI Designer",
    department: "Product",
    location: "Remote",
    status: "Open",
  },
];

const jobs = ref([]);
const searchKeyword = ref("");
const selectedStatus = ref("All");
const closeModalButton = ref(null);
const editingJobId = ref(null);
const form = ref({
  title: "",
  department: "",
  location: "",
  status: "Open",
});

const modalTitle = computed(() => {
  return editingJobId.value ? "Edit Job" : "Add New Job";
});

const filteredJobs = computed(() => {
  const keyword = searchKeyword.value.toLowerCase();

  return jobs.value.filter((job) => {
    const matchesTitle = job.title.toLowerCase().includes(keyword);
    const matchesStatus = selectedStatus.value === "All" || job.status === selectedStatus.value;

    return matchesTitle && matchesStatus;
  });
});

const getStatusBadgeClass = (status) => {
  return status === "Open" ? "text-bg-success" : "text-bg-secondary";
};

const saveJobs = () => {
  localStorage.setItem(storageKey, JSON.stringify(jobs.value));
};

const resetForm = () => {
  editingJobId.value = null;
  form.value = {
    title: "",
    department: "",
    location: "",
    status: "Open",
  };
};

const closeModal = () => {
  closeModalButton.value?.click();
};

const openAddJob = () => {
  resetForm();
};

const openEditJob = (job) => {
  editingJobId.value = job.id;
  form.value = {
    title: job.title,
    department: job.department,
    location: job.location,
    status: job.status,
  };
};

const handleSubmitJob = () => {
  if (editingJobId.value) {
    const jobIndex = jobs.value.findIndex((job) => job.id === editingJobId.value);

    if (jobIndex !== -1) {
      jobs.value[jobIndex] = {
        id: editingJobId.value,
        title: form.value.title,
        department: form.value.department,
        location: form.value.location,
        status: form.value.status,
      };
    }

    saveJobs();
    resetForm();
    closeModal();
    return;
  }

  const newJob = {
    id: Date.now(),
    title: form.value.title,
    department: form.value.department,
    location: form.value.location,
    status: form.value.status,
  };

  jobs.value.push(newJob);
  saveJobs();
  resetForm();
  closeModal();
};

const deleteJob = (jobId) => {
  const isConfirmed = confirm("Yakin ingin menghapus job ini?");

  if (!isConfirmed) {
    return;
  }

  jobs.value = jobs.value.filter((job) => job.id !== jobId);
  saveJobs();
};

const loadJobs = () => {
  const savedJobs = localStorage.getItem(storageKey);

  if (!savedJobs) {
    localStorage.setItem(storageKey, JSON.stringify(dummyJobs));
    jobs.value = dummyJobs;
    return;
  }

  jobs.value = JSON.parse(savedJobs);
  localStorage.setItem(storageKey, JSON.stringify(jobs.value));
};

onMounted(() => {
  loadJobs();
});
</script>

<template>
  <MainLayout>
    <div class="d-flex justify-content-between align-items-center mb-4">
      <div>
        <h1 class="h3 fw-semibold mb-1">Jobs</h1>
        <p class="text-secondary mb-0">Daftar posisi yang sedang diproses.</p>
      </div>

      <button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#jobModal" @click="openAddJob">
        <i class="bi bi-plus-lg me-2"></i>
        Tambah Job
      </button>
    </div>

    <div class="card table-card border-0 shadow-sm">
      <div class="card-body border-bottom">
        <div class="row g-3 align-items-center">
          <div class="col-md-6 col-lg-4">
            <label for="searchJob" class="form-label small fw-semibold text-secondary"> Search Jobs </label>
            <div class="input-group">
              <span class="input-group-text bg-white">
                <i class="bi bi-search"></i>
              </span>
              <input id="searchJob" v-model="searchKeyword" type="text" class="form-control" placeholder="Cari job title..." />
            </div>
          </div>

          <div class="col-md-6 col-lg-3">
            <label for="statusFilter" class="form-label small fw-semibold text-secondary"> Filter Status </label>
            <select id="statusFilter" v-model="selectedStatus" class="form-select">
              <option value="All">All</option>
              <option value="Open">Open</option>
              <option value="Closed">Closed</option>
            </select>
          </div>
        </div>
      </div>

      <div class="table-responsive">
        <table class="table table-hover align-middle mb-0">
          <thead>
            <tr>
              <th>Job Title</th>
              <th>Department</th>
              <th>Location</th>
              <th>Status</th>
              <th class="text-end">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="job in filteredJobs" :key="job.id">
              <td class="fw-medium">{{ job.title }}</td>
              <td>{{ job.department }}</td>
              <td>{{ job.location }}</td>
              <td>
                <span class="badge rounded-pill" :class="getStatusBadgeClass(job.status)">
                  {{ job.status }}
                </span>
              </td>
              <td class="text-end">
                <button class="btn btn-light btn-sm me-2" type="button" data-bs-toggle="modal" data-bs-target="#jobModal" @click="openEditJob(job)">
                  <i class="bi bi-pencil"></i>
                </button>
                <button class="btn btn-outline-danger btn-sm" type="button" @click="deleteJob(job.id)">
                  <i class="bi bi-trash"></i>
                </button>
              </td>
            </tr>

            <tr v-if="filteredJobs.length === 0">
              <td colspan="5" class="text-center text-secondary py-4">Data job tidak ditemukan.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div id="jobModal" class="modal fade" tabindex="-1" aria-labelledby="jobModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content border-0 shadow">
          <form @submit.prevent="handleSubmitJob">
            <div class="modal-header">
              <h2 id="jobModalLabel" class="modal-title h5 fw-semibold">{{ modalTitle }}</h2>
              <button ref="closeModalButton" type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>

            <div class="modal-body">
              <div class="mb-3">
                <label for="jobTitle" class="form-label">Job Title</label>
                <input id="jobTitle" v-model="form.title" type="text" class="form-control" placeholder="Contoh: Backend Developer" required />
              </div>

              <div class="mb-3">
                <label for="department" class="form-label">Department</label>
                <input id="department" v-model="form.department" type="text" class="form-control" placeholder="Contoh: Engineering" required />
              </div>

              <div class="mb-3">
                <label for="location" class="form-label">Location</label>
                <input id="location" v-model="form.location" type="text" class="form-control" placeholder="Contoh: Jakarta" required />
              </div>

              <div>
                <label for="status" class="form-label">Status</label>
                <select id="status" v-model="form.status" class="form-select" required>
                  <option value="Open">Open</option>
                  <option value="Closed">Closed</option>
                </select>
              </div>
            </div>

            <div class="modal-footer">
              <button type="button" class="btn btn-light" data-bs-dismiss="modal">Batal</button>
              <button type="submit" class="btn btn-primary">
                <i class="bi bi-save me-2"></i>
                {{ editingJobId ? "Update Job" : "Simpan Job" }}
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </MainLayout>
</template>
