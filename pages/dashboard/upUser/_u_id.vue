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



  
      <v-card-title class="text-center">Edit User</v-card-title>
      
      <v-card-text>
  
        <form  @submit.prevent="upUser()" >
 
          <v-text-field  v-model="user.email"  type="email" :counter="50" label="Email" 
            > </v-text-field>
            <v-text-field  v-model="user.username"  :counter="50" label="Username" 
            > </v-text-field>
  
            <v-text-field  v-model="user.firstname"  :counter="50" label="firstname" > 
            </v-text-field>
            <v-text-field  v-model="user.lastname"  :counter="50" label="Lastname" > 
            </v-text-field>
            <v-select  v-model="user.u_stat"  :items="items"  label="user Status"  ></v-select>
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
    name: 'UpUser',
    middleware: 'auth',
      // eslint-disable-next-line vue/order-in-components
      el:'#UpUser',
        data(){ 
          return {
            items: ['NotApproved', 'Approved', 'Admin'],
            showAlert: false,
            errorAlert: false,
            errorMessage:'',
            regisMessage:'',
            user: {
                u_id: '',
                email: '',
                username: '',
                firstname: '',
                lastname: '',
                u_state: '',
            },
                }
          },
          mounted() {
            this.getUser()
            // console.log(this.showAlert)
            // console.log(this.errorAlert)
          },
          updated() {
            // console.log(this.showAlert)
            // this.ShowAlert
          },
          methods: {
            async getUser(){
                try {
            const res = await this.$axios.get(`/user/${this.$route.params.u_id}`);
            
            res.data.forEach(val => {
                
                this.user.u_id = val.u_id;
                this.user.email = val.email;
                this.user.username = val.username;
                this.user.firstname = val.firstname;
                this.user.lastname = val.lastname;
                this.user.u_stat= val.u_stat;
            });
            
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
            },
      async upUser() {
          await this.$axios.patch(`users/adupUser/${this.user.u_id}`,{
          email: this.user.email,
          username: this.user.username,
          firstname: this.user.firstname,
          lastname: this.user.lastname,
          password: this.user.password,
          u_stat: this.user.u_stat,
          
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