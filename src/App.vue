<template>
  <div id="app"  class="small-container">
    <h1>Employees</h1>
    <employee-form @add:employee="addEmployee" />
    <employee-table v-bind:employees="employees" @edit:employee="editEmployee" @delete:employee="deleteEmployee" />
  </div>
</template>

<script>
import EmployeeTable from './components/EmployeeTable.vue'
import EmployeeForm from './components/EmployeeForm.vue'

export default {
  name: 'app',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: []
    }
  },
  mounted() {
    this.getEmployees();
  },

  // Asynchronous CRUD API call equivalent of simple CRUD methods
  methods: {
    async getEmployees() {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        const data = await response.json();
        this.employees = data;
      } catch(error) {
        console.log(error);
      }
    },
    async addEmployee(employee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        });
        const data = await response.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        console.error(error);
      }
    },
    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
        const data = await response.json()
        this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
      } catch (error) {
        console.error(error)
      }
    },
    async deleteEmployee(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: "DELETE"
        });
        this.employees = this.employees.filter(employee => employee.id !== id);
      } catch (error) {
        console.error(error);
      }
    },
  }

  // Simple CRUD Methods
  // methods: {
  //   addEmployee(employee) {
  //     const id =
  //         this.employees.length > 0
  //       ? this.employees[this.employees.length - 1].id + 1
  //       : 1;
  //     const newEmployee = { ...employee, id };
  //     this.employees = [...this.employees, newEmployee];
  //   },
  //   editEmployee(id, updatedEmployee) {
  //     this.employees = this.employees.map(employee =>
  //       employee.id === id ? updatedEmployee : employee
  //     )
  //   },
  //   deleteEmployee(id) {
  //     this.employees = this.employees.filter(
  //       employee => employee.id !== id
  //     );
  //   },
  // }
}
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 720px;
  }
</style>
