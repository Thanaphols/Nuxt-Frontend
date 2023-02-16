<template>

    <v-card class="mx-auto my-12"  max-width="500">
      <!-- Alert Success -->
      <v-alert v-show="showAlert"  dense outlined   type="success">
          {{regisMessage.message}}
          
      </v-alert> 
      <!-- Error Success -->
      <v-alert v-for="(item, i) in errorMessage.data"  v-show="errorAlert" :key="i" dense outlined  type="error">
          {{item}}
      </v-alert> 
  
      <v-card-title class="text-center">Update</v-card-title>
      
      <v-card-text>
  
        <form  @submit.prevent="addUser()" >
  
          <v-text-field  v-model="user.email"  type="email" :counter="50" label="Email" 
            > </v-text-field>
            <v-text-field  v-model="user.username"  :counter="50" label="Username" 
            > </v-text-field>
            
  
            <v-text-field  v-model="user.lastname"  :counter="50" label="Lastname" > 
            </v-text-field>
  
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
    name: 'ReGis',
      // eslint-disable-next-line vue/order-in-components
        data(){ 
          return {
            showAlert: false,
            errorAlert: false,
            errorMessage:'',
            regisMessage:'',
            user: {
                      email:"",
                      username:"",
                      lastname:"",
                      password:"",
                    },
                }
          },
          mounted() {
            // console.log(this.showAlert)
            // console.log(this.errorAlert)
          },
          updated() {
            // console.log(this.showAlert)
            // this.ShowAlert
          },
          methods: {
      async addUser() {
          await this.$axios.post(`/users/addUser/`,{
          email: this.user.email,
          username: this.user.username,
          lastname: this.user.lastname,
          password: this.user.password,
          
        })
        .then((response) => {
          this.errorAlert = false
          this.showAlert = true
          // eslint-disable-next-line no-console
          console.log(response);
          this.regisMessage =  response.data;
        })
        .catch((error)=> {
          // eslint-disable-next-line no-console
          console.error(error);
          this.showAlert = false
          this.errorAlert = true
          this.errorMessage =  error.response;
         
        })
       
      },
       resetForm(){
                this.user.email="";
                this.user.username="";
                this.user.lastname="";
                this.user.password="";
      }
                  }
  }
  
  
  </script>