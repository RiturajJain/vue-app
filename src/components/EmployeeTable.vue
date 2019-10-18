<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">
      No employees
    </p>
    <table v-else>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" :key="employee.id">
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{ employee.name }}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{ employee.email }}</td>
          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Save</button>
            <button class="muted-button" @click="cancelEdit(employee)">Cancel</button>
          </td>
          <td v-else>
            <button class="update-btn" @click="editMode(employee)">Edit</button>
            <button class="delete-btn" @click="$emit('delete:employee', employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'employee-table',
    props: {
      employees: Array,
    },
    data() {
      return {
        editing: null
      }
    },
    methods: {
      editMode(employee) {
        this.cachedEmployee = Object.assign({}, employee)
        this.editing = employee.id
      },
      editEmployee(employee) {
        if (employee.name === '' || employee.email === '') return
        this.$emit('edit:employee', employee.id, employee);
        this.editing = null;
      },
      cancelEdit(employee) {
        Object.assign(employee, this.cachedEmployee)
        this.editing = null;
      }
    }
  }
</script>

<style scoped>
  button {
    margin: 0 1rem 0 0;
  }

  table {
    table-layout: fixed;
  }

  .empty-table {
    color: red;
    font-size: 1.5rem;
    text-align: center;
  }
  .update-btn {
    background: #039dfc;
    border: 0;
  }
  .delete-btn {
    background: #d11733;
    border: 0;
  }

</style>