<template>
    
  <v-card  class="mx-auto   my-12"  max-width="500">
    <!-- Alert Success -->
    
    
    <v-alert v-show="showAlert"  dense outlined   type="success">
        {{loginMessage.message}}
    </v-alert> 
    <!-- Error Success -->
    <v-alert v-show="errorAlert"  dense outlined  type="error">
        {{errorMessage}}
    </v-alert> 

    <v-card-title class="text-center">Login</v-card-title>
    
    <v-card-text>

      <form  @submit.prevent="logIn" >

        <v-text-field  v-model="user.email"  type="email" :counter="50" label="Email" 
          > </v-text-field>

          <v-text-field  v-model="user.password" type="password"  :counter="8"  label="Password" 
          > </v-text-field>

          <v-card-actions  >
            
      <v-btn  type="submit"  >
          submit
      </v-btn>
      <v-btn @click="resetForm()">
          clear
      </v-btn>
    </v-card-actions>
        </form>
     
    </v-card-text>

    <v-divider class="mx-4"></v-divider>

    

    <div class="text-center">
  
  </div>
  </v-card>
</template>

<script>

export default {
  name: 'Login',
  layout: 'default',
      data(){ 
        return {
          showAlert: false,
          errorAlert: false,
          errorMessage:'',
          loginMessage:'',
          user: {
                    email:"",
                    password:"",
                  },
              }
        },
        mounted() {
          // console.log(this.showAlert)
          // eslint-disable-next-line no-console
          // console.log(this.errorAlert)
        },
        updated() {
          // eslint-disable-next-line no-console
          // console.log(this.showAlert)
          // this.ShowAlert
        },
        methods: {
    async logIn(e) {
      e.preventDefault()
      try {
        const response = await this.$auth.loginWith('local', {data: this.user, })
        
         // eslint-disable-next-line no-console
        console.log(response)
        // eslint-disable-next-line no-console
        console.log(this.$auth.user)
        this.showAlert = true
        this.errorAlert = false
         this.loginMessage =  response.data;
         
         setTimeout(async () =>{  
          await location.reload()
          //  this.$router.push('/')
         }, response)
          
         
       } catch (error) {
          // eslint-disable-next-line no-console
          console.log(error);
        this.showAlert = false
        this.errorAlert = true
        this.errorMessage =  error.response.data.message;

      }

      //   await this.$axios.post(`http://localhost:9753/api/auth/login/`,{
      //   email: this.user.email,
      //   password: this.user.password,
        
      // })
      // .then((response) => {
      //   this.errorAlert = false
      //   this.showAlert = true
      //   // eslint-disable-next-line no-console
      //   console.log(response);
      //   this.loginMessage =  response.data;
        
      // })
      // .catch((error)=> {
      //   // eslint-disable-next-line no-console
      //   console.error(error);
      //   this.showAlert = false
      //   this.errorAlert = true
      //   this.errorMessage =  error.response;
       
      // })
     
    },
     resetForm(){
              this.user.email="";
              this.user.password="";
    }
                }
}


</script>