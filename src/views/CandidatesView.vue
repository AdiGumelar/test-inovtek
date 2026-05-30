<script setup>
import { computed, onMounted, ref } from "vue";
import MainLayout from "../layouts/MainLayout.vue";

const storageKey = "recruitment_candidates";

const stages = ["Applied", "Interview", "Hired"];

const dummyCandidates = [
  {
    id: 1,
    name: "Ayu Lestari",
    role: "Frontend Developer",
    email: "ayu.lestari@email.com",
    stage: "Applied",
  },
  {
    id: 2,
    name: "Bima Pratama",
    role: "UI Designer",
    email: "bima.pratama@email.com",
    stage: "Interview",
  },
  {
    id: 3,
    name: "Citra Amelia",
    role: "HR Generalist",
    email: "citra.amelia@email.com",
    stage: "Hired",
  },
  {
    id: 4,
    name: "Dewi Kartika",
    role: "Frontend Developer",
    email: "dewi.kartika@email.com",
    stage: "Applied",
  },
];

const candidates = ref([]);

const candidatesByStage = computed(() => {
  return stages.map((stage) => ({
    name: stage,
    items: candidates.value.filter((candidate) => candidate.stage === stage),
  }));
});

const normalizeCandidates = (candidateList) => {
  return candidateList.map((candidate) => ({
    ...candidate,
    stage: stages.includes(candidate.stage) ? candidate.stage : "Applied",
  }));
};

const loadCandidates = () => {
  const savedCandidates = localStorage.getItem(storageKey);

  if (!savedCandidates) {
    localStorage.setItem(storageKey, JSON.stringify(dummyCandidates));
    candidates.value = dummyCandidates;
    return;
  }

  candidates.value = normalizeCandidates(JSON.parse(savedCandidates));
  localStorage.setItem(storageKey, JSON.stringify(candidates.value));
};

onMounted(() => {
  loadCandidates();
});
</script>

<template>
  <MainLayout>
    <div class="d-flex flex-column flex-md-row justify-content-between align-items-md-center gap-3 mb-4">
      <div>
        <h1 class="h3 fw-semibold mb-1">Candidates</h1>
        <p class="text-secondary mb-0">Pipeline kandidat berdasarkan status recruitment.</p>
      </div>
    </div>

    <div class="row g-3">
      <div v-for="column in candidatesByStage" :key="column.name" class="col-lg-4">
        <div class="kanban-column h-100">
          <div class="d-flex align-items-center justify-content-between mb-3">
            <h2 class="h6 fw-semibold mb-0">{{ column.name }}</h2>
            <span class="badge text-bg-light">{{ column.items.length }}</span>
          </div>

          <div class="d-grid gap-3">
            <div v-for="candidate in column.items" :key="candidate.id" class="card candidate-card border-0 shadow-sm">
              <div class="card-body">
                <div class="d-flex align-items-start justify-content-between gap-3">
                  <div>
                    <h3 class="h6 fw-semibold mb-1">{{ candidate.name }}</h3>
                    <p class="text-secondary small mb-2">{{ candidate.role }}</p>
                    <p class="text-secondary small mb-0">
                      <i class="bi bi-envelope me-1"></i>
                      {{ candidate.email }}
                    </p>
                  </div>

                  <span class="candidate-avatar d-inline-flex align-items-center justify-content-center">
                    {{ candidate.name.charAt(0) }}
                  </span>
                </div>
              </div>
            </div>

            <div v-if="column.items.length === 0" class="empty-kanban text-center text-secondary p-4">Belum ada kandidat.</div>
          </div>
        </div>
      </div>
    </div>
  </MainLayout>
</template>
