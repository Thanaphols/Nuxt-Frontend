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
          {{loginMessage.message}}
      </v-alert> 
      <!-- Error Success -->
      <v-alert v-show="errorAlert"  dense outlined  type="error">
          {{errorMessage.message}}
      </v-alert>
  
      <form enctype="multipart/form-data"  @submit.prevent="upEquipment()" >
        <v-card >
          <v-row align="center">
           
            <v-col sm="12">
            <v-card-title class="justify-center"  >
            <h2 >Edit Equipment</h2>
          </v-card-title>
          </v-col>
          </v-row>
          
          <v-card-text >
            <v-row >
              <v-col sm-12 >
                <v-img 
              v-if=" eq.i_img === null" 
              
              :aspect-ratio="1"
              height="250"
              width="100%"
              cover
              :src="require(`~/assets/images/noimg.png`)"
            ></v-img>
            <v-img
              v-else
              :aspect-ratio="1"
              height="250"
              width="100%"
              cover
              :src="require(`~/assets/images/${eq.i_img}`)"
            ></v-img>
          </v-col>
            </v-row>
            <v-row >
            <!--autocomplete -->
         <v-col sm="12" >
            <v-text-field   v-model="eq.i_name"   type="text" label="Equipment Name" outlined  ></v-text-field>
         </v-col>

         <v-col sm="12">
          <v-file-input  ref="Eqimg" v-model="files" show-size counter chips multiple label="Images" name="image"></v-file-input>
         </v-col>

         <v-col sm="9" >
         
            <v-autocomplete  v-model="category" :items="cate"  item-value="c_id"  item-text="c_name"   dense  filled label="Category"></v-autocomplete>
         </v-col>
         <v-col sm="3" >
            <v-text-field v-model="eq.i_qty" min="0"  type="number" label="Quatity" outlined  ></v-text-field>
         </v-col>

         </v-row>
         </v-card-text>
  
         <v-row >
          <v-col sm="4">
            <v-card-actions  >
              <div class="justtify-center" >
              <v-btn class="primary "  depressed  type="submit">submit</v-btn>
            </div>
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
      files: null,
      showAlert: false,
      errorAlert: false,
      errorMessage:'',
      loginMessage:'',
      data:[],
      eq:{
        i_name:'',
        i_qty:'',
        i_category:'',
        i_img:'noimg.png',
      },
      
      cate:[],
      category:'',
    }
  },
  mounted() {
            this.getInv()
            this.getCate()
            
            // console.log(this.showAlert)
            // console.log(this.errorAlert)
          },
  
  methods : {
  
       
  
    async upEquipment() {
      if(this.files != null) {
      try {
        console.log(222)
            const formData = new FormData()
              formData.append('file', this.files[0])
              formData.append('i_name', this.eq.i_name)
              formData.append('i_qty', this.eq.i_qty)
              formData.append('c_id', this.category)
              const res =  await this.$axios.patch(`/upEq/${this.$route.params.e_id}`, formData)
              this.data = res.data
              this.errorAlert = false
              this.showAlert = true
              this.loginMessage =  this.data;
          } catch (error) {
          
          console.error(error);
            this.showAlert = false;
            this.errorAlert = true
            this.errorMessage =  error.response.data;
            }
          }else{
            try {
            console.log(111)
              const res =  await this.$axios.patch(`/upEq/${this.$route.params.e_id}`,{
              i_name : this.eq.i_name,
              i_qty : this.eq.i_qty,
              c_id : this.category,
              })
              console.log(this.data)
              this.data = res.data
              this.errorAlert = false
              this.showAlert = true
            
              
              this.loginMessage =  this.data;
          } catch (error) {
          
          console.error(error);
            this.showAlert = false;
            this.errorAlert = true
            this.errorMessage =  error.response.data;
            }
          }
             
      
        // await this.$axios.patch(`/inv/upInv/${this.$route.params.e_id}`,{
        // i_name: this.eq.i_name,
        // i_qty: this.eq.i_qty,
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
        //   this.errorMessage =  error.response.data;
         
        // })
       
      },
  
      async getInv() {
      try {
        const res = await this.$axios.get(`/inv/${this.$route.params.e_id}`);
        res.data.forEach(value => {
            this.eq.i_name = value.i_name;
            this.eq.i_qty = value.i_qty;
            this.eq.i_category = value.i_category;
            this.eq.i_img = value.i_img;
            this.category = value.i_category;
        });
       
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
    
    }
  }
  
  </script>