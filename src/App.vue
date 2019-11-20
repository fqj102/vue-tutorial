<template>
    <div
            id="app"
            class="small-container"
    >
        <h1>Employees</h1>

        <employee-form @add:employee="addEmployee"/>

        <employee-table
                :employees="employees"
                @delete:employee="deleteEmployee"
                @edit:employee="editEmployee"
        />
    </div>
</template>

<script>
    import EmployeeTable from '@/components/EmployeeTable.vue'
    import EmployeeForm from '@/components/EmployeeForm.vue'

    export default {
        name: "app",
        components: {
            EmployeeTable,
            EmployeeForm,
        },
        data() {
            return {
                employees: []
            }
        },

        mounted() {
            console.log("########### App.vue mounted ########");
            this.getEmployees()
        },

        methods: {
            async getEmployees() {
                try {
                    const response = await fetch('https://jsonplaceholder.typicode.com/users')
                    const data = await response.json()
                    this.employees = data
                } catch (error) {
                    console.error(error)
                }
            },

            async addEmployee(employee) {
                try {
                    /*
                    const response = await fetch('https://jsonplaceholder.typicode.com/users', {
                        method: 'POST',
                        body: JSON.stringify(employee),
                        headers: {"Content-type": "application/json; charset=UTF-8"}
                    })
                     */
                    //const data = await response.json()
                    //this.employees = [...this.employees, data]
                    let id=0;
                    if(this.employees.length>0){
                        const last = this.employees[this.employees.length-1];
                        console.log("------last------",last);
                        id=last.id+1
                    }
                    employee.id = id;
                    this.employees = [...this.employees, employee]
                } catch (error) {
                    console.error(error)
                }
            },

            async editEmployee(id, updatedEmployee) {
                console.log("-------------id-------",id);
                console.log("-------------updatedEmployee-----",updatedEmployee);
                try {
                    //const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
                    //    method: 'PUT',
                    //    body: JSON.stringify(updatedEmployee),
                    //    headers: {"Content-type": "application/json; charset=UTF-8"}
                    //})
                    //const data = await response.json()
                    //this.employees = this.employees.map(employee => employee.id === id ? employee : employee)
                    const emp = this.employees.filter(employee => employee.id === id)[0];
                    console.log('------emp',emp)
                    const newEmp = {emp,...updatedEmployee};
                    console.log('------emp2------',newEmp);
                    this.employees.map(employee => employee.id === id ? newEmp : employee)

                } catch (error) {
                    console.error(error)
                }
            },

            async deleteEmployee(id) {
                try {
                    //await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
                    //    method: 'DELETE'
                    //})
                    this.employees = this.employees.filter(employee => employee.id !== id)
                } catch (error) {
                    console.error(error)
                }
            },
        },
    }
</script>

<style>
    button {
        background: #009435;
        border: 1px solid #009435;
    }

    button:hover,
    button:active,
    button:focus {
        background: #32a95d;
        border: 1px solid #32a95d;
    }

    .small-container {
        max-width: 680px;
    }
</style>
