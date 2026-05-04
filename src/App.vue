<template>
  <div id="app">

    <!-- Header -->
    <div class="text-white text-center py-4" style="background-color: #1a3c5e;">
      <h1 class="fw-bold fs-3 mb-1">Employee Management System</h1>
      <p class="mb-0 small" style="color: #a8c4de;">
        Built with Vue.js · Axios · MockAPI
      </p>
    </div>

    <!-- Info Cards -->
    <div class="container my-4">
      <h5 class="fw-bold mb-1">How This App Works</h5>
      <p class="text-muted small mb-3">
        This app uses four HTTP methods via Axios to perform CRUD operations on MockAPI.
      </p>

      <div class="row g-3">

        <div class="col-md-3 col-sm-6">
          <div class="card h-100 shadow-sm border-0 text-center p-2"
               style="border-top: 4px solid #0d6efd !important;">
            <div class="card-body pb-2">
              <span class="badge bg-primary mb-2 px-3">POST</span>
              <h6 class="fw-bold">Create</h6>
              <p class="text-muted small mb-0">
                Sends employee data to MockAPI and saves a new record.
              </p>
            </div>
          </div>
        </div>

        <div class="col-md-3 col-sm-6">
          <div class="card h-100 shadow-sm border-0 text-center p-2"
               style="border-top: 4px solid #198754 !important;">
            <div class="card-body pb-2">
              <span class="badge bg-success mb-2 px-3">GET</span>
              <h6 class="fw-bold">Read</h6>
              <p class="text-muted small mb-0">
                Fetches all records from MockAPI and displays in a table.
              </p>
            </div>
          </div>
        </div>

        <div class="col-md-3 col-sm-6">
          <div class="card h-100 shadow-sm border-0 text-center p-2"
               style="border-top: 4px solid #fd7e14 !important;">
            <div class="card-body pb-2">
              <span class="badge mb-2 px-3 text-white"
                    style="background-color: #fd7e14;">PUT</span>
              <h6 class="fw-bold">Update</h6>
              <p class="text-muted small mb-0">
                Updates an existing employee record using their ID.
              </p>
            </div>
          </div>
        </div>

        <div class="col-md-3 col-sm-6">
          <div class="card h-100 shadow-sm border-0 text-center p-2"
               style="border-top: 4px solid #dc3545 !important;">
            <div class="card-body pb-2">
              <span class="badge bg-danger mb-2 px-3">DELETE</span>
              <h6 class="fw-bold">Delete</h6>
              <p class="text-muted small mb-0">
                Removes an employee record from MockAPI by ID.
              </p>
            </div>
          </div>
        </div>

      </div>
    </div>

    <!-- CRUD Components -->
    <div class="container pb-5">
      <EmployeeForm />
      <EmployeeList />
      <UpdateEmployee />
      <DeleteEmployee />
    </div>

  </div>
</template>

<script>
import EmployeeForm from './components/EmployeeForm.vue'
import EmployeeList from './components/EmployeeList.vue'
import UpdateEmployee from './components/UpdateEmployee.vue'
import DeleteEmployee from './components/DeleteEmployee.vue'

export default {
  name: 'App',
  components: { EmployeeForm, EmployeeList, UpdateEmployee, DeleteEmployee }
}
</script>

<style>
body {
  background-color: #eef1f6;
  font-family: Arial, sans-serif;
}
</style>