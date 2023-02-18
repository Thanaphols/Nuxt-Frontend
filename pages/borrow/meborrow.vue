<!-- eslint-disable vue/valid-v-slot -->
<template>
    <v-row class="justtify-center mt-1" >
      <v-col cols="12" sm="12"  md="12" >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText"
    elevation="11" outlined >
   
      <template #top>
        <v-text-field
          v-model="search"
          label="Search (UPPER CASE ONLY)"
          class="mx-4 "
        ></v-text-field>
      </template>

    <template #item.img="{ item }">
        
        <v-img
        v-if=" item.i_img === null" 
        height="100"
        width="100"
        cover
        :src="require(`~/assets/images/noimg.png`)"
      ></v-img>
      <v-img
        v-else
        height="100"
        width="100"
        cover
        :src="require(`~/assets/images/${item.i_img}`)"
      ></v-img>
          
      </template>
      
      <!-- <template #item.actions="{ item }">
        <v-btn icon  outlined  class="error"   @click="deBorrow(item.b_id,item.b_qty,item.i_id)">
            <v-icon >
        mdi-delete
      </v-icon>
        </v-btn>
    </template>

    <template #item.Return="{ item }">
        <v-btn icon  outlined  class="teal lighten-1"   @click="reBorrow(item.b_id,item.b_qty,item.i_id)">
          <svg-icon type="mdi" :path="path"></svg-icon>
        </v-btn>
       
    </template> -->

      
    </v-data-table>
    </v-col>
    </v-row>
    
  </template>
   
  
  <script>
  // import SvgIcon from '@jamescoyle/vue-icon';
  // import { mdiKeyboardReturn } from '@mdi/js';
   export default {
    name: "MeBorrow",
    components: {
		// SvgIcon
	},
    
    middleware: 'auth',
    data () {
      return {
        // path: mdiKeyboardReturn,
        u_id: '',
        data:[],
        up:[],
        delete:[],
        user:[],
        re:[],
        search: '',
        b_id: '',
        eq:[],
       
      }
    },
    
    computed: {
        
      headers () {
        return [
          
          { text: 'Borrow (id)', value: 'b_id', filter: value => {
              if (!this.b_id) return true
              return value < parseInt(this.b_id)
            }, },
          { text: 'UserName', value: 'username' },
          { text: 'Inventory Name', value: 'i_name'  },
          { text: 'Equipment Images', value: 'img'   },
          { text: 'Category', value: 'c_name' },
          { text: 'Quetity', value: 'b_qty', sortable: false },
          { text: 'Borrow Date', value: 'b_date', sortable: false },
          { text: 'Borrow Status', value: 'b_stat', sortable: false },
          // { text: 'Actions', value: 'actions', sortable: false },
          // { text: 'Return', value: 'Return', sortable: false },
          
        ]
      },
    },
    mounted() {
          
          this.user = this.$auth.user
           this. u_id = this.user.u_id
           this.getmeBorrow()
        },
    methods: {
      filterOnlyCapsText (value, search, item) {
        return value != null &&
          search != null &&
          typeof value === 'string' &&
          value.toString().toLocaleUpperCase().includes(search)
      },

      async deBorrow(id,qty,id2) {
            try {
            const res = await this.$axios.delete(`/borrow/deBorrow/${id}/${qty}/${id2}`);
            this.delete = res.data;
            this.$router.push('/borrow/meBorrow/')
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);
            }
        },

    async reBorrow(id,qty,id2) {
        try {
        const res = await this.$axios.patch(`/borrow/return/${id}/${qty}/${id2}`);
        this.re = res.data;
        this.$router.push('/borrow/meBorrow/')
        } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);

        }
    },

      async getmeBorrow() {
            try {
            const res = await this.$axios.get(`/borrow/borrow/${this.u_id}`);
            this.data = res.data;
            // eslint-disable-next-line no-console
            console.log(this.data);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },
    },
  }
  </script>