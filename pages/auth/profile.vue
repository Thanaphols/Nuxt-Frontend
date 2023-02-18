<template>

    <v-card class="mx-auto my-12"  max-width="500">
      <!-- Alert Success -->
      <v-alert v-show="showAlert"  dense outlined   type="success">
          {{regisMessage.message}}
          
      </v-alert> 
      <!-- Error Success -->
      <v-alert v-show="errorAlert" dense outlined  type="error">
          {{errorMessage}}
      </v-alert> 
  
      <v-card-title class="text-center">Profile</v-card-title>
      
      <v-card-text>
  
        <form  @submit.prevent="upUser()" >
          
          <v-text-field  v-model="data.email"  type="email" :counter="50" label="Email" 
            > </v-text-field>

            <v-text-field  v-model="data.username"  :counter="50" label="Username" 
            > </v-text-field>
            <v-text-field  v-model="data.firstname"  :counter="50" label="Firstname" 
            > </v-text-field>
  
            <v-text-field  v-model="data.lastname"  :counter="50" label="Lastname" > 
            </v-text-field>
  
            <v-text-field  v-model="data.password" type="password"  :counter="8"  label="Password" 
            > </v-text-field>
            <v-card-actions  >
              
        <v-btn  type="submit"  >
            submit
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
    name: 'Profile',
    middleware: 'auth',
      // eslint-disable-next-line vue/order-in-components
        data(){ 
          return {
            showAlert: false,
            errorAlert: false,
            errorMessage:'',
            regisMessage:'',
            user: [],
            data:{
              u_id: '',
              email: '',
              username: '',
              firstname: '',
              lastname: '',
              password: '',
            },
                }
          },
          mounted() {
           this.user = this.$auth.user
           this.getuser()
          },
          methods: {
      async upUser() {
          await this.$axios.patch(`users/upUser/${this.user.u_id}`,{
          email: this.data.email,
          username: this.data.username,
          firstname: this.data.firstname,
          lastname: this.data.lastname,
          password: this.data.password,
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
          this.errorMessage =  error.response.data.message;
         
        })
       
      },
      async getuser(){
        const id = this.user.u_id;
        try {
          const res = await this.$axios.get(`/user/${id}`)
          res.data.forEach(val => {
            this.data.email = val.email
              this.data.username= val.username
              this.data.firstname= val.firstname
              this.data.lastname= val.lastname
              this.data.password= val.password
          });
          // console.log(this.data)

        } catch (error) {
          
        }
      }
                  }
  }
  
  
  </script>