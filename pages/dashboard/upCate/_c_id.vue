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
  
      <form  @submit.prevent="upCate()" >
        <v-card >
          <v-row align="center">
            <v-col sm="4"></v-col>
            <v-col sm="6">
            <v-card-title  >
            <h2 class="text-center">Edit Category</h2>
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
        c_id: '',
        c_name: '',
      },
      category:'',
    }
  },
  mounted() {
            this.getCate()
            // console.log(this.showAlert)
            // console.log(this.errorAlert)
          },
  
  methods : {
  
       
  
    async upCate() {
          await this.$axios.patch(`/cate/upCate/${this.$route.params.c_id}`,{
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
  
     
  
    async getCate() {
      try {
        const res = await this.$axios.get(`/cate/${this.$route.params.c_id}`);
        res.data.forEach(val => {
          this.cate.c_id = val.c_id;
          this.cate.c_name = val.c_name;
        });

        // eslint-disable-next-line no-unused-expressions
        
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
  
      }
    },
      
    resetForm(){
                this.eq.i_name="";
                this.eq.i_qty="0";
      },
    }
  }
  
  </script>