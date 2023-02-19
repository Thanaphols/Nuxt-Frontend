<template>
  
  <v-card class="mx-auto my-12" elevation="11" light outlined  max-width="500">
    <!-- Alert Success -->
    <v-alert v-show="showAlert"  dense outlined   type="success">
        {{regisMessage}}
        
    </v-alert> 
    <!-- Error Success -->
    <v-alert  v-show="errorMessage"  dense outlined  type="error">
        {{errorMessage}}
    </v-alert> 

    <v-card-title class="justify-center  " sm12 xs12 >
      <div class=" while--text" >สมัครสมาชิก</div>
    </v-card-title>
    
    <v-card-text>

      <form  @submit.prevent="addUser()" >

        <v-text-field  v-model="user.email"  type="email" :counter="50"  label="อีเมล"   outlined
          > </v-text-field>
          <v-text-field  v-model="user.username"  :counter="50" label="Username"  outlined
          > </v-text-field>
          
          <v-row>
            <v-col cols="12" sm="6" >
              <v-text-field  v-model="user.firstname"  :counter="50" label="ชื่อจริง"  outlined > 
          </v-text-field>
            </v-col>
            <v-col cols="12" sm="6" >
            <v-text-field  v-model="user.lastname"  :counter="50" label="นามสกุล"  outlined> 
          </v-text-field>
            </v-col>
          </v-row>

          <v-text-field  v-model="user.password" type="password" max=8  :counter="8"   label="รหัสผ่าน"  outlined
          > </v-text-field>
          <v-card-actions  >
            
          <v-btn  type="submit"  >
              ยืนยัน
          </v-btn>
          <v-btn @click="resetForm()">
              ล้างข้อมูล
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
                    firstname:"",
                    lastname:"",
                    password:"",
                  },
         
              }
        },
        mounted() {
          // console.log(this.errorMessage)
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
        firstname: this.user.firstname,
        lastname: this.user.lastname,
        password: this.user.password,
        
      })
      .then((response) => {
        this.errorAlert = false
        this.showAlert = true
        // eslint-disable-next-line no-console
        console.log(response);
        this.regisMessage =  response.data.message;
      })
      .catch((error)=> {
        // eslint-disable-next-line no-console
        console.error(error);
        this.showAlert = false
        this.errorAlert = true
        this.errorMessage =  error.response.data.message;
       
      })
     
    },
     resetForm(){
              this.user.email="";
              this.user.username="";
              this.user.firstname="";
              this.user.lastname="";
              this.user.password="";
    }
                }
}


</script>