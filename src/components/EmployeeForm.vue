<template>
    <div id="employee-form">
        <!--event listeners -->
        <form @submit.prevent="handleSubmit">
            <label>Employee name</label>
            <!--write input into component-->
            <!--config input 
            <input v-model ="employee.name" type="text" />
            <input v-model ="employee.email" type="text" />
            after click submit, focus back to input
            -->
            <input 
            ref="first" 
            type="text"
            :class="{'has-erros':submitting && invalidName}"
            v-model ="employee.name"
            @focus="clearStatus"
            @keypress="clearStatus" />

            <label>Employee email</label>
            <input
            type="text"
            :class="{'has-error':submitting && invalidEmail}"
            v-model ="employee.email"
            @focus="clearStatus"
            />

            <!--message box-->
            <p v-if="error && submitting" class="error-message">
                !Please fill out all requried fields
            </p>
            <p v-if="success" class="success-message">
                Employee sucessfullly added
            </p>

            <button>Add Employee</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'employee-form',
    data(){
        return {
            //add state 
            submitting: false, //Issubmitted
            error: false, //sth went wrong
            success: false, //sth went well

            employee:{
                name: '',
                email: '',
            },
        }
    },
    methods:{
        //1. add clearStatus()
        //2. update handleSubmit()        
        handleSubmit(){
           //console.log('testing handleSubmit')
           //start submit
           this.submitting = true
           this.clearStatus()

           //check both input correctly
           if(this.invalidName || this.invalidEmail){
               this.error = true
               return
           }
           
           //emit employee
           this.$emit('add:employee',this.employee)

           //focus back to input
           this.$refs.first.focus()

           //reset params
           this.error = false
           this.success = true
           this.submitting = false

        },
        clearStatus(){
            this.success = false
            this.error = false
        },
    },
    //validate form
    computed: {
        invalidName() {
            return this.employee.name === ''
        },
        invalidEmail(){
            return this.employee.email === ''  
        },
    },
}
</script>

<style scoped>
  form {
      margin-bottom: 2rem;
  }
  /* format error or success messenge */
  [class*='-message']{
      font-weight: 500;
  }

  .error-message {
      color: #d33c40;
  }

  .success-message {
      color: #32a95d;
  }
</style>