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
          {{loginMessage}}
      </v-alert> 
      <!-- Error Success -->
      <v-alert v-show="errorAlert"  dense outlined  type="error">
          {{errorMessage}}
      </v-alert>
  
      <form  enctype="multipart/form-data" @submit.prevent="addEquipment()" >
        <v-card >
          <v-row align="center">
            <v-col sm="4"></v-col>
            <v-col sm="6">
            <v-card-title  >
            <h2 class="text-center">เพิ่มสินค้า</h2>
          </v-card-title>
          </v-col>
          </v-row>
          
          <v-card-text>
            <v-row >
             
  
            <!--autocomplete -->
         <v-col sm="12" >
            <v-text-field   v-model="equipment.i_name" max="qty"  type="text" label="Equipment Name" outlined  ></v-text-field>
         </v-col>
         <v-col sm="12">
          <v-file-input  ref="Eqimg" v-model="files" show-size counter chips multiple label="Images" name="image"></v-file-input>
         </v-col>
         <v-col sm="9" >
            <v-autocomplete  v-model="category" :items="cate" item-value="c_id" item-text="c_name"    dense  filled label="Category">
            
            </v-autocomplete>
         </v-col>
         <v-col sm="3" >
            <v-text-field    v-model="equipment.i_qty" min="0"  type="number" label="Quatity" outlined  ></v-text-field>
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
      equipment:{
        i_name: '',
        i_qty: '',
        i_category: '',
      },
         cate:[],
         category:'',
    }
  },
  mounted() {
            this.getCate()
            // console.log(this.showAlert)
            // console.log(this.errorAlert)
          },
  
  methods : {
  
       
  
    async addEquipment() {

      try {
            const formData = new FormData()
              formData.append('file', this.files[0])
              formData.append('i_name', this.equipment.i_name)
              formData.append('i_qty', this.equipment.i_qty)
              formData.append('c_id', this.category)
              const res =  await this.$axios.post(`/upload/`, formData)
              this.data = res.data
              console.log(this.data)
              this.errorAlert = false
              this.showAlert = true
            // eslint-disable-next-line no-console
            console.log(this.data);
            this.loginMessage =  this.data.data.message;
            } catch (error) {
                        // eslint-disable-next-line no-console
          console.error(error);
          this.showAlert = false
          this.errorAlert = true
          this.errorMessage =  error.response.data.message;
            }


        //   await this.$axios.post(`/inv/addInv`,{
        // i_name: this.equipment.i_name,
        // i_qty: this.equipment.i_qty,
        // c_id: this.category,
          
        // })
        // .then((response) => {
        //   this.errorAlert = false
        //   this.showAlert = true
        //   // eslint-disable-next-line no-console
        //   console.log(response);
        //    this.loginMessage =  response.data;
        // })
        // .catch((error)=> {
        //   // eslint-disable-next-line no-console
        //   console.error(error);
        //   this.showAlert = false
        //   this.errorAlert = true
        //   this.errorMessage =  error.response.data.message;
         
        // })
       
      },
  
  
    async getCate() {
      try {
        const res = await this.$axios.get(`/cate`);
        this.cate = res.data;
        // eslint-disable-next-line no-console
        console.log(this.cate);
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e); 
  
      }
    },
    resetForm(){
                this.equipment.i_name="";
                this.equipment.i_qty="";
                this.category="";
      },
    }
  }
  
  </script>