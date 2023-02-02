<template>
    <v-row justify="center mt-1">
      <v-col
        cols="12"
        sm="10"
        md="8"
        lg="6"
      >
      <!-- Alert Success -->
      <v-alert v-show="showAlert"  dense outlined   type="success">
          {{loginMessage.message}}
      </v-alert> 
      <!-- Error Success -->
      <v-alert v-show="errorAlert"  dense outlined  type="error">
          {{errorMessage.message}}
      </v-alert>
  
      <form  @submit.prevent="addCate()" >
        <v-card >
          <v-row align="center">
            <v-col sm="4"></v-col>
            <v-col sm="6">
            <v-card-title  >
            <h2 class="text-center">Add Category</h2>
          </v-card-title>
          </v-col>
          </v-row>
          
          <v-card-text >
            <v-row >
         <v-col sm="12" >
            <v-text-field    v-model="cate.c_name" min="0"  type="text" label="Category Name" outlined  ></v-text-field>
         </v-col>

         </v-row>
         </v-card-text>
  
         <v-row >
          <v-col sm="4">
          </v-col>
          <v-col sm="4">
            <v-card-actions  >
              <v-btn  type="submit">submit</v-btn>
              <v-btn @click="resetForm()">
                  clear
              </v-btn>
        </v-card-actions>  
          </v-col>
         </v-row>
         
         </v-card>
        </form>
      </v-col>
    </v-row>
  </template>
   
  
  <script>
  
  export default {
    name: "AddEquipment",
    middleware: 'auth',
    data() {
    return {
      showAlert: false,
      errorAlert: false,
      errorMessage:'',
      loginMessage:'',
      cate:{
        c_name: '',
      },
    }
  },
  mounted() {
            // console.log(this.showAlert)
            // console.log(this.errorAlert)
          },
  
  methods : {
  
       
  
    async addCate() {
          await this.$axios.post(`/cate/addCate/`,{
        c_name: this.cate.c_name,
          
        })
        .then((response) => {
          this.errorAlert = false
          this.showAlert = true
          // eslint-disable-next-line no-console
          console.log(response);
           this.loginMessage =  response.data;
        })
        .catch((error)=> {
          // eslint-disable-next-line no-console
          console.error(error);
          this.showAlert = false
          this.errorAlert = true
          this.errorMessage =  error.response.data;
         
        })
       
      },
      
    resetForm(){
                this.cate.c_name="";
      },
    }
  }
  
  </script>