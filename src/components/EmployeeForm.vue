<template>
  <div class="card border-0 shadow-sm rounded-4 overflow-hidden">

    <!-- Header -->
    <div class="card-header d-flex justify-content-between align-items-center py-3 px-4 text-white bg-primary">
      <div class="d-flex align-items-center gap-3">
        <div class="bg-white bg-opacity-25 rounded-3 p-2 d-flex align-items-center justify-content-center">
          <svg width="18" height="18" fill="none" viewBox="0 0 24 24">
            <path d="M12 5v14M5 12h14" stroke="white" stroke-width="2.5" stroke-linecap="round"/>
          </svg>
        </div>
        <div>
          <h6 class="mb-0 fw-bold">Add New Employee</h6>
          <small class="text-white-50" style="font-size:11px;">Fill the form to create a record</small>
        </div>
      </div>
      <span class="badge bg-white text-primary fw-bold px-3 py-2 rounded-pill">POST</span>
    </div>

    <!-- Body -->
    <div class="card-body p-4">
      <div class="row g-3">

        <div class="col-md-6">
          <label class="form-label fw-semibold text-secondary small text-uppercase">Full Name</label>
          <div class="input-group">
            <span class="input-group-text bg-primary bg-opacity-10 border-primary border-opacity-25 text-primary">👤</span>
            <input class="form-control" v-model="form.name" placeholder="e.g. Priya Sharma" />
          </div>
        </div>

        <div class="col-md-6">
          <label class="form-label fw-semibold text-secondary small text-uppercase">Designation</label>
          <div class="input-group">
            <span class="input-group-text bg-primary bg-opacity-10 border-primary border-opacity-25 text-primary">💼</span>
            <input class="form-control" v-model="form.designation" placeholder="e.g. Software Engineer" />
          </div>
        </div>

        <div class="col-md-6">
          <label class="form-label fw-semibold text-secondary small text-uppercase">Department</label>
          <div class="input-group">
            <span class="input-group-text bg-primary bg-opacity-10 border-primary border-opacity-25 text-primary">🏢</span>
            <input class="form-control" v-model="form.department" placeholder="e.g. Engineering" />
          </div>
        </div>

        <div class="col-md-6">
          <label class="form-label fw-semibold text-secondary small text-uppercase">Salary (₹)</label>
          <div class="input-group">
            <span class="input-group-text bg-primary bg-opacity-10 border-primary border-opacity-25 text-primary">₹</span>
            <input class="form-control" type="number" v-model="form.salary" placeholder="e.g. 85000" />
          </div>
        </div>

      </div>

      <!-- Success Alert -->
      <transition name="fade">
        <div v-if="successMsg" class="alert alert-success d-flex align-items-center gap-2 mt-3 mb-0 py-2 px-3 rounded-3">
          <span>✅</span>
          <span class="fw-semibold small">{{ successMsg }}</span>
        </div>
      </transition>

      <!-- Buttons -->
      <div class="d-flex justify-content-end gap-2 mt-4">
        <button class="btn btn-outline-secondary px-4 rounded-3" @click="resetForm">
          Reset
        </button>
        <button class="btn btn-primary px-5 fw-bold rounded-3 d-flex align-items-center gap-2"
                @click="submitForm" :disabled="loading">
          <span v-if="loading" class="spinner-border spinner-border-sm"></span>
          Add Employee
        </button>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
const API = 'https://69f80a56dd0c226688ee1cc9.mockapi.io/employees'

export default {
  name: 'EmployeeForm',
  data() {
    return {
      form: { name: '', designation: '', department: '', salary: '' },
      loading: false,
      successMsg: ''
    }
  },
  methods: {
    async submitForm() {
      if (!this.form.name || !this.form.designation) return
      this.loading = true
      await axios.post(API, this.form)
      this.successMsg = 'Employee added successfully!'
      this.loading = false
      this.resetForm()
      setTimeout(() => { this.successMsg = '' }, 3000)
    },
    resetForm() {
      this.form = { name: '', designation: '', department: '', salary: '' }
    }
  }
}
</script>

<style scoped>
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
</style>