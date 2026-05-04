<template>
  <div class="card border-0 shadow-sm rounded-4 overflow-hidden">

    <!-- Header -->
    <div class="card-header d-flex justify-content-between align-items-center py-3 px-4 text-white bg-success">
      <div class="d-flex align-items-center gap-3">
        <div class="bg-white bg-opacity-25 rounded-3 p-2 d-flex align-items-center justify-content-center">
          <svg width="18" height="18" fill="none" viewBox="0 0 24 24">
            <rect x="3" y="3" width="18" height="4" rx="2" fill="white" opacity="0.9"/>
            <rect x="3" y="10" width="18" height="4" rx="2" fill="white" opacity="0.7"/>
            <rect x="3" y="17" width="18" height="4" rx="2" fill="white" opacity="0.5"/>
          </svg>
        </div>
        <div>
          <h6 class="mb-0 fw-bold">Employee Records</h6>
          <small class="text-white-50" style="font-size:11px;">All employees fetched from API</small>
        </div>
      </div>
      <div class="d-flex align-items-center gap-2">
        <span class="badge bg-white bg-opacity-25 text-white rounded-pill px-3">{{ list.length }} records</span>
        <span class="badge bg-white text-success fw-bold px-3 py-2 rounded-pill">GET</span>
      </div>
    </div>

    <!-- Search -->
    <div class="card-body border-bottom pb-3 pt-3 px-4 bg-white">
      <div class="input-group" style="max-width:320px;">
        <span class="input-group-text bg-success bg-opacity-10 border-success border-opacity-25 text-success">🔍</span>
        <input class="form-control" v-model="search" placeholder="Search by name or department…" />
      </div>
    </div>

    <!-- Table -->
    <div class="card-body p-0 bg-white">

      <!-- Loading -->
      <div v-if="loading" class="text-center py-5">
        <div class="spinner-border text-success mb-2" role="status"></div>
        <div class="text-muted small">Fetching employees…</div>
      </div>

      <!-- Empty -->
      <div v-else-if="filtered.length === 0" class="text-center py-5">
        <div class="fs-1 mb-2">👥</div>
        <p class="text-muted fw-semibold mb-0">No employees found</p>
      </div>

      <!-- Table -->
      <div v-else class="table-responsive">
        <table class="table table-hover align-middle mb-0">
          <thead class="table-success">
            <tr>
              <th class="ps-4 py-3 text-success fw-bold small text-uppercase" style="width:70px;">ID</th>
              <th class="py-3 text-success fw-bold small text-uppercase">Name</th>
              <th class="py-3 text-success fw-bold small text-uppercase">Designation</th>
              <th class="py-3 text-success fw-bold small text-uppercase">Department</th>
              <th class="py-3 pe-4 text-success fw-bold small text-uppercase text-end">Salary</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in filtered" :key="item.id">
              <td class="ps-4 py-3">
                <span class="badge bg-secondary bg-opacity-10 text-secondary fw-semibold">{{ item.id }}</span>
              </td>
              <td class="py-3">
                <div class="d-flex align-items-center gap-2">
                  <div class="rounded-circle d-flex align-items-center justify-content-center text-white fw-bold flex-shrink-0"
                       :class="`bg-${avatarColor(item.name)}`" style="width:34px;height:34px;font-size:13px;">
                    {{ item.name.charAt(0).toUpperCase() }}
                  </div>
                  <span class="fw-semibold">{{ item.name }}</span>
                </div>
              </td>
              <td class="py-3 text-muted small">{{ item.designation }}</td>
              <td class="py-3">
                <span class="badge bg-success bg-opacity-10 text-success rounded-pill px-3 py-2 fw-semibold">
                  {{ item.department }}
                </span>
              </td>
              <td class="py-3 pe-4 text-end fw-semibold text-dark">₹ {{ Number(item.salary).toLocaleString() }}</td>
            </tr>
          </tbody>
        </table>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios'
const API = 'https://69f80a56dd0c226688ee1cc9.mockapi.io/employees'
const AVATAR_COLORS = ['primary', 'success', 'warning', 'danger', 'info', 'secondary']

export default {
  name: 'EmployeeList',
  data() {
    return { list: [], search: '', loading: true }
  },
  computed: {
    filtered() {
      const q = this.search.toLowerCase()
      return this.list.filter(e =>
        e.name.toLowerCase().includes(q) ||
        (e.department || '').toLowerCase().includes(q)
      )
    }
  },
  methods: {
    avatarColor(name) { return AVATAR_COLORS[name.charCodeAt(0) % AVATAR_COLORS.length] }
  },
  async mounted() {
    const resp = await axios.get(API)
    this.list = resp.data
    this.loading = false
  }
}
</script>