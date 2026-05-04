<template>
  <div class="card border-0 shadow-sm rounded-4 overflow-hidden">

    <!-- Header -->
    <div class="card-header d-flex justify-content-between align-items-center py-3 px-4 text-white bg-warning">
      <div class="d-flex align-items-center gap-3">
        <div class="bg-white bg-opacity-25 rounded-3 p-2 d-flex align-items-center justify-content-center">
          <svg width="18" height="18" fill="none" viewBox="0 0 24 24">
            <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7" stroke="white" stroke-width="2" stroke-linecap="round"/>
            <path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z" stroke="white" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </div>
        <div>
          <h6 class="mb-0 fw-bold text-dark">Update Employee</h6>
          <small class="text-dark text-opacity-75" style="font-size:11px;">Click Edit on any row to modify</small>
        </div>
      </div>
      <span class="badge bg-white text-warning fw-bold px-3 py-2 rounded-pill">PUT</span>
    </div>

    <!-- Table -->
    <div class="card-body p-0 bg-white">

      <div v-if="loading" class="text-center py-5">
        <div class="spinner-border text-warning mb-2" role="status"></div>
        <div class="text-muted small">Loading…</div>
      </div>

      <div v-else class="table-responsive">
        <table class="table table-hover align-middle mb-0">
          <thead class="table-warning">
            <tr>
              <th class="ps-4 py-3 text-warning-emphasis fw-bold small text-uppercase" style="width:70px;">ID</th>
              <th class="py-3 text-warning-emphasis fw-bold small text-uppercase">Name</th>
              <th class="py-3 text-warning-emphasis fw-bold small text-uppercase">Designation</th>
              <th class="py-3 text-warning-emphasis fw-bold small text-uppercase">Department</th>
              <th class="py-3 text-warning-emphasis fw-bold small text-uppercase text-end">Salary</th>
              <th class="py-3 pe-4 text-warning-emphasis fw-bold small text-uppercase text-center">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in list" :key="item.id"
                :class="editData.id === item.id ? 'table-warning' : ''">
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
                <span class="badge bg-warning bg-opacity-25 text-warning-emphasis rounded-pill px-3 py-2 fw-semibold">
                  {{ item.department }}
                </span>
              </td>
              <td class="py-3 text-end fw-semibold">₹ {{ Number(item.salary).toLocaleString() }}</td>
              <td class="py-3 pe-4 text-center">
                <button class="btn btn-sm btn-warning fw-semibold rounded-3 px-3" @click="editItem(item)">
                  ✏️ Edit
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Edit Panel -->
    <transition name="slide-down">
      <div v-if="editData.id" class="card-footer bg-warning bg-opacity-10 border-top border-warning border-opacity-50 px-4 py-4">

        <div class="d-flex align-items-center justify-content-between mb-3">
          <span class="badge bg-warning text-dark px-3 py-2 rounded-pill fw-bold">
            Editing — Employee ID: {{ editData.id }}
          </span>
          <button class="btn btn-sm btn-outline-secondary rounded-3" @click="editData = { id: null }">
            ✕ Cancel
          </button>
        </div>

        <div class="row g-3 mb-3">
          <div class="col-md-6">
            <label class="form-label fw-semibold text-secondary small text-uppercase">Name</label>
            <input class="form-control" v-model="editData.name" placeholder="Full Name" />
          </div>
          <div class="col-md-6">
            <label class="form-label fw-semibold text-secondary small text-uppercase">Designation</label>
            <input class="form-control" v-model="editData.designation" placeholder="Designation" />
          </div>
          <div class="col-md-6">
            <label class="form-label fw-semibold text-secondary small text-uppercase">Department</label>
            <input class="form-control" v-model="editData.department" placeholder="Department" />
          </div>
          <div class="col-md-6">
            <label class="form-label fw-semibold text-secondary small text-uppercase">Salary (₹)</label>
            <div class="input-group">
              <span class="input-group-text">₹</span>
              <input class="form-control" type="number" v-model="editData.salary" placeholder="Salary" />
            </div>
          </div>
        </div>

        <div class="d-flex justify-content-end gap-2">
          <button class="btn btn-outline-secondary px-4 rounded-3" @click="editData = { id: null }">Cancel</button>
          <button class="btn btn-warning fw-bold px-5 rounded-3 d-flex align-items-center gap-2"
                  @click="updateData" :disabled="saving">
            <span v-if="saving" class="spinner-border spinner-border-sm"></span>
            💾 Update Employee
          </button>
        </div>

      </div>
    </transition>

  </div>
</template>

<script>
import axios from 'axios'
const API = 'https://69f80a56dd0c226688ee1cc9.mockapi.io/employees'
const AVATAR_COLORS = ['primary', 'success', 'warning', 'danger', 'info', 'secondary']

export default {
  name: 'UpdateEmployee',
  data() {
    return { list: [], loading: true, saving: false, editData: { id: null } }
  },
  methods: {
    avatarColor(name) { return AVATAR_COLORS[name.charCodeAt(0) % AVATAR_COLORS.length] },
    async fetchData() {
      this.loading = true
      const resp = await axios.get(API)
      this.list = resp.data
      this.loading = false
    },
    editItem(item) { this.editData = { ...item } },
    async updateData() {
      this.saving = true
      await axios.put(`${API}/${this.editData.id}`, this.editData)
      await this.fetchData()
      this.editData = { id: null }
      this.saving = false
    }
  },
  mounted() { this.fetchData() }
}
</script>

<style scoped>
.slide-down-enter-active, .slide-down-leave-active { transition: all 0.28s ease; }
.slide-down-enter-from, .slide-down-leave-to { opacity: 0; transform: translateY(-8px); }
</style>