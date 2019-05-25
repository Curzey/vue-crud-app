<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>  
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
          <!-- Name field with editing state -->
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{ employee.name }}</td>

          <!-- Email field with editing state -->
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{ employee.email }}</td>

          <!-- Button field with editing state -->
          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(employee.id)">Edit</button>
            <button @click="$emit('delete:employee', employee.id)">Delete</button>
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
        editing: null,
      }
    },
    methods: {
      editMode(id) {
        this.editing = id
      },

      editEmployee(employee) {
        // if any field is empty, bail
        if (employee.name === '' || employee.email === '') return;

        // emit updated employee
        this.$emit('edit:employee', employee.id, employee);

        // resit editing state
        this.editing = null;
      }
    }
  }
</script>

<style scoped></style>