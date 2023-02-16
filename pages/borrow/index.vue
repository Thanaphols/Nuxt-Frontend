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
          <h2  >Select Equipment to Borrow</h2>
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
    category: 0,
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
          console.log(this.equment)
          this.user = this.$auth.user
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
        await this.$axios.post(`/borrow/addBorrow`,{
        u_id: this.user.u_id,
        u_stat: this.user.u_stat,
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


  async getEqu(id) {
    try {
      console.log(111)
      const res = await this.$axios.get(`/inv/cate/${id}`);
      this.eq = res.data;
      // eslint-disable-next-line no-console
      console.log(this.eq);
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
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);

    }
  },
  resetForm(){
              this.i_qty="";
              this.qty="";
    },
  }
}

</script>