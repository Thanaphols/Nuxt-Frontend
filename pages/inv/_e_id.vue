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
  
      <form  @submit.prevent="upEquipment()" >
        <v-card >
          <v-row align="center">
            <v-col sm="4"></v-col>
            <v-col sm="6">
            <v-card-title  >
            <h2 class="text-center">แก้ไขอุปกรณ์</h2>
          </v-card-title>
          </v-col>
          </v-row>
          
          <v-card-text >
            <v-row >
             
              
            <!--autocomplete -->
         <v-col sm="12" >
            <v-text-field   v-model="eq.i_name"   type="text" label="Equipment Name" outlined  ></v-text-field>
         </v-col>
         <v-col sm="9" >
         
            <v-autocomplete  v-model="category" :items="cate"  item-value="c_id"  item-text="c_name"   dense  filled label="Category"></v-autocomplete>
         </v-col>
         <v-col sm="3" >
            <v-text-field    v-model="eq.i_qty" min="0"  type="number" label="Quatity" outlined  ></v-text-field>
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
      eq:{
        i_name:'',
        i_qty:'',
        i_category:'',
      },
      cate:{
        c_id: '',
        c_name: '',
      },
      category:'',
    }
  },
  mounted() {
            this.getCate()
            this.getInv()
            // console.log(this.showAlert)
            // console.log(this.errorAlert)
          },
  
  methods : {
  
       
  
    async upEquipment() {
          await this.$axios.patch(`/inv/upInv/${this.$route.params.e_id}`,{
        i_name: this.eq.i_name,
        i_qty: this.eq.i_qty,
        c_id: this.category,
          
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
  
      async getInv() {
      try {
        const res = await this.$axios.get(`/inv/${this.$route.params.e_id}`);
        res.data.forEach(value => {
            this.eq.i_name = value.i_name;
            this.eq.i_qty = value.i_qty;
            this.eq.i_category = value.i_category;
        });
        this.eq = res.data;
       
        // eslint-disable-next-line no-console
       // console.log(this.eq);
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
  
      }
    },
  
    async getCate() {
      try {
        const res = await this.$axios.get(`/cate`);
        this.cate = res.data;
        // eslint-disable-next-line no-console
        // console.log(this.cate);

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