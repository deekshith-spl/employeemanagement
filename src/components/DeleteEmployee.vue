<template>
  <div class="card border-0 shadow-sm rounded-4 overflow-hidden">

    <!-- Header -->
    <div class="card-header d-flex justify-content-between align-items-center py-3 px-4 text-white bg-danger">
      <div class="d-flex align-items-center gap-3">
        <div class="bg-white bg-opacity-25 rounded-3 p-2 d-flex align-items-center justify-content-center">
          <svg width="18" height="18" fill="none" viewBox="0 0 24 24">
            <path d="M3 6h18M8 6V4h8v2M19 6l-1 14H6L5 6" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M10 11v6M14 11v6" stroke="white" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </div>
        <div>
          <h6 class="mb-0 fw-bold">Delete Employee</h6>
          <small class="text-white-50" style="font-size:11px;">Permanently remove a record</small>
        </div>
      </div>
      <span class="badge bg-white text-danger fw-bold px-3 py-2 rounded-pill">DELETE</span>
    </div>

    <!-- Table -->
    <div class="card-body p-0 bg-white">

      <div v-if="loading" class="text-center py-5">
        <div class="spinner-border text-danger mb-2" role="status"></div>
        <div class="text-muted small">Loading…</div>
      </div>

      <div v-else-if="list.length === 0" class="text-center py-5">
        <div class="fs-1 mb-2">🗑️</div>
        <p class="text-muted fw-semibold mb-0">All records have been deleted</p>
      </div>

      <div v-else class="table-responsive">
        <table class="table table-hover align-middle mb-0">
          <thead class="table-danger">
            <tr>
              <th class="ps-4 py-3 text-danger fw-bold small text-uppercase" style="width:70px;">ID</th>
              <th class="py-3 text-danger fw-bold small text-uppercase">Name</th>
              <th class="py-3 text-danger fw-bold small text-uppercase">Designation</th>
              <th class="py-3 text-danger fw-bold small text-uppercase">Department</th>
              <th class="py-3 text-danger fw-bold small text-uppercase text-end">Salary</th>
              <th class="py-3 pe-4 text-danger fw-bold small text-uppercase text-center">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in list" :key="item.id"
                :class="{ 'opacity-50': deletingId === item.id }">
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
                <span class="badge bg-danger bg-opacity-10 text-danger rounded-pill px-3 py-2 fw-semibold">
                  {{ item.department }}
                </span>
              </td>
              <td class="py-3 text-end fw-semibold">₹ {{ Number(item.salary).toLocaleString() }}</td>
              <td class="py-3 pe-4 text-center">
                <button class="btn btn-sm btn-outline-danger fw-semibold rounded-3 px-3 d-inline-flex align-items-center gap-1"
                        @click="confirmItem = item"
                        :disabled="deletingId === item.id">
                  <span v-if="deletingId === item.id" class="spinner-border spinner-border-sm"></span>
                  <span v-else>🗑️</span>
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

    </div>

    <!-- Confirm Modal -->
    <transition name="modal-fade">
      <div v-if="confirmItem"
           class="position-fixed top-0 start-0 w-100 h-100 d-flex align-items-center justify-content-center"
           style="background:rgba(0,0,0,0.45);z-index:1050;backdrop-filter:blur(3px);"
           @click.self="confirmItem = null">
        <div class="card border-0 shadow-lg rounded-4 p-4 text-center" style="max-width:360px;width:90%;">
          <div class="fs-1 mb-2">⚠️</div>
          <h6 class="fw-bold mb-1">Delete Employee?</h6>
          <p class="text-muted small mb-4">
            You are about to permanently delete<br>
            <strong class="text-dark">{{ confirmItem?.name }}</strong>
            <span class="badge bg-secondary bg-opacity-10 text-secondary ms-1">ID: {{ confirmItem?.id }}</span>
            <br>This action cannot be undone.
          </p>
          <div class="d-flex gap-2">
            <button class="btn btn-outline-secondary flex-fill rounded-3 fw-semibold" @click="confirmItem = null">
              Cancel
            </button>
            <button class="btn btn-danger flex-fill rounded-3 fw-bold" @click="doDelete">
              Yes, Delete
            </button>
          </div>
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
  name: 'DeleteEmployee',
  data() {
    return { list: [], loading: true, deletingId: null, confirmItem: null }
  },
  methods: {
    avatarColor(name) { return AVATAR_COLORS[name.charCodeAt(0) % AVATAR_COLORS.length] },
    async fetchData() {
      this.loading = true
      const res = await axios.get(API)
      this.list = res.data
      this.loading = false
    },
    async doDelete() {
      const id = this.confirmItem.id
      this.deletingId = id
      this.confirmItem = null
      await axios.delete(`${API}/${id}`)
      this.list = this.list.filter(i => i.id !== id)
      this.deletingId = null
    }
  },
  mounted() { this.fetchData() }
}
</script>

<style scoped>
.modal-fade-enter-active, .modal-fade-leave-active { transition: opacity 0.25s ease; }
.modal-fade-enter-from, .modal-fade-leave-to { opacity: 0; }
</style>