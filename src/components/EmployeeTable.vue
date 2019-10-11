<template>
    <div id="employee-table">
        <!--print no employee -->
        <p v-if="employees.length < 1" class="empty-table">
            No employees
        </p>
        <table v-else>
            <thead>
            <tr>
            <th>Employee name</th>
            <th>Employee email</th>
            <th>Actions</th></tr></thead> <!---->
            <tbody>
            <tr v-for="employee in employees" :key="employee.id">
                <!-- listen to editing id-->
                <td v-if="editing === employee.id">
                    <input type="text" v-model="employee.name">
                </td>
                <td v-else>{{employee.name}}</td>

                <!-- listen to editin id-->
                <td v-if="editing === employee.id">
                    <input type="text" v-model="employee.email">
                </td>
                <td v-else>{{employee.email}}</td>

              <td v-if="editing === employee.id">
                  <button @click="editEmployee(employee)">Save</button>
                  <!--@click="editing = null" -->
                  <button @click="cancelEdit(employee)" class="muted-button">Cancel</button>
              </td>
              <td v-else>
                  <button @click="editMode(employee)">Edit</button><!--remove id from emplyee.id-->
                  <button @click="$emit('delete:employee',employee.id)">Delete</button><!---->
              </td>

              <!--update name and email
              <td>{{employee.name}}</td>
              <td>{{employee.email}}</td> -->
            </tr>
            <!--
            <tr>
            <th>Employee name</th>
            <th>Employee email</th></tr></thead>
            <tbody>
            <tr v-for="employee in employees" :key="employee.id">
              <td>{{employee.name}}</td>
              <td>{{employee.email}}</td>
            </tr>
            -->
            <!--
            <tr><td>Richard</td><td>richard@vue.com</td></tr>
            <tr><td>Bertram</td><td>bertram@vue.com</td></tr>
            <tr><td>Dinesh</td><td>dinesh@vue.com</td></tr>
            -->
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'employee-table',
    props:{
        employees: Array,
    },
    //edit mode
    data(){
        return {
            editing:null,
        }
    },
    methods:{
        editMode(employee){ 
            this.cachedEmployee = Object.assign({},employee)
            this.editing = employee.id
        },
        /*editMode(id){ 
            this.editing = id
        }, */
        editEmployee(employee){
            if(employee.name === '' || employee.email === '') return
            this.$emit('edit:employee',employee.id,employee)
            this.editing = null
        },
        cancelEdit(employee){
            Object.assign(employee, this.cachedEmployee)
            this.editing = null;
        },
    }
}
</script>

<style scoped></style>