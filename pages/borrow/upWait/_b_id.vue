<template>
    <v-row class="justify-center mt-1">
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
  
      <form  @submit.prevent="addBorrow()" >
        <v-card >
          <v-row >
            <v-col sm="12">
            <v-card-title class="text-center"  >
            <h2  >Edit Borrow Request </h2>
          </v-card-title>
          </v-col>
          </v-row>
          
          <v-card-text>
            <v-row >
             
  
            <!--autocomplete -->
         <v-col sm="6">
          <v-autocomplete
          v-model="category" :items="cate" item-value="c_id"  item-text="c_name"   dense  filled label="Category"
          @change="getEqu(category)" ></v-autocomplete>
         </v-col>
  
         <v-col sm="6" >
          <v-autocomplete
          v-model="equment" :items="eq" item-value="i_id"  item-text="i_name"   dense  filled label="Equipment"
          @change="handleChange" ></v-autocomplete>
         </v-col>
  
         <v-col cols="12" sm="6" >
            <v-text-field   v-model="i_qty" max="qty"  type="number" label="Quatity" outlined  ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" >
            <v-text-field   v-model="qty" type="number" label="Emaining" outlined disabled ></v-text-field>
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
    name: "Borrow",
    middleware: 'auth',
    data() {
    return {
      showAlert: false,
      errorAlert: false,
      errorMessage:'',
      loginMessage:'',
      equment:'',
      category:'',
      user:[],
      qty:'',
      i_qty:'',
      eq: [],
      cate:[],
      
    }
  },
  mounted() {
            // this.getEqu()
            this.getCate()
            // console.log(this.showAlert)
            // console.log(this.errorAlert)
            // eslint-disable-next-line no-console
            
            this.user = this.$auth.user
            // this.category = this.$route.params.b_id;
            // console.log(this.$route.params.b_id)
            
            this.getBorrow()
            
          },
  
  methods : {
  
        // eslint-disable-next-line camelcase
        handleChange(id) {
         // eslint-disable-next-line camelcase, no-console
        //  console.log(`Selected: ${id}`)
        this.eq.forEach((val) => {
          if (val.i_id === id) {
            // eslint-disable-next-line no-console
            console.log(val)
            this.qty = val.i_qty
          }
        })
        // Perform action based on selected value
         },
  
    async addBorrow() {
          await this.$axios.patch(`/borrow/upBorrow/${this.$route.params.b_id}`,{
          u_id: this.user.u_id,
          i_id: this.equment,
          c_id: this.category,
          i_qty: this.i_qty,
          // จำนวนคงเหลือ qty
          qty: this.qty,
          
        })
        .then((response) => {
          this.errorAlert = false
          this.showAlert = true
          // eslint-disable-next-line no-console
          console.log(response);
          this.loginMessage =  response.data.message;
        })
        .catch((error)=> {
          // eslint-disable-next-line no-console
          console.error(error);
          this.showAlert = false
          this.errorAlert = true
          this.errorMessage =  error.response.data.message;
         
        })
       
      },
    async getCate() {
      try {
        const res = await this.$axios.get(`/cate`);
        this.cate = res.data;
        // eslint-disable-next-line no-console
        // console.log(this.cate);
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
  
      }
    },
    async getEqu(id) {
      console.log(id)
    try {
      const res = await this.$axios.get(`/inv/cate/${id}`);
      this.eq = res.data;
      // eslint-disable-next-line no-console
      // console.log(this.eq);
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);

    }
  },
    async getBorrow(){
      const id = this.$route.params.b_id;
        try {
          const res = await this.$axios.get(`/borrow/${id}`);
          res.data.forEach(val => {
            this.category = val.i_category
            this.equment = val.i_id
            this.i_qty = val.b_qty
            this.qty = val.i_qty
            this.getEqu(this.category)
           
          });
          
        } catch (error) {
          console.log(error);
        }
    },
    resetForm(){
                this.i_qty="";
                this.qty="";
      },
    }
  }
  
  </script>