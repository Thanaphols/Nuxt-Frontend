<!-- eslint-disable vue/valid-v-slot -->
<template>
    <v-row class="justify-center mt-1">
      <v-col cols="12" sm="12"  md="12" >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
   
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
      
      <template #item.actions="{ item }">

       
        
        <v-btn  icon  outlined  class=" primary"   @click="upWait(item.b_id,item.b_qty,item.i_id)">
          <svg-icon type="mdi" :path="path2"></svg-icon>
        </v-btn> 
        
        <v-btn  icon  outlined  class="mr-1 teal error"    @click="deWait(item.b_id)">
          <v-icon >
        mdi-delete
          </v-icon>
        </v-btn>
    
    </template>

    

      
    </v-data-table>
    </v-col>
    </v-row>
    
  </template>
   
  
  <script>
  import SvgIcon from '@jamescoyle/vue-icon';
  import { mdiKeyboardReturn,mdiCheckBold  } from '@mdi/js';
   export default {
    name: "AppBorrow",
    components: {
		SvgIcon
	},
    
    middleware: 'auth',
    data () {
      return {
        path: mdiKeyboardReturn,
        path2: mdiCheckBold,
        u_id: '',
        data:[],
        up:[],
        delete:[],
        re:[],
        search: '',
        b_id: '',
       
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
          { text: 'Actions', value: 'actions', sortable: false },
          
        ]
      },
    },
    mounted() {
          this.getWait()
        },
    methods: {
      filterOnlyCapsText (value, search, item) {
        return value != null &&
          search != null &&
          typeof value === 'string' &&
          value.toString().toLocaleUpperCase().includes(search)
      },

      async upWait(id,qty,id2) {
        
        try {
        const res = await this.$axios.patch(`/admin/upWait/${id}/${qty}/${id2}`);
        this.up = res.data;
        // eslint-disable-next-line no-console
        // console.log(this.up);
        this.$router.go('')
        } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);

        }
    },

      async deWait(id) {
        
            try {
            const res = await this.$axios.delete(`/admin/deWait/${id}`);
            this.delete = res.data;
            // eslint-disable-next-line no-console
            // console.log(this.delete);
            setTimeout(async () =>{  
            await location.reload()
            //  this.$router.push('/')
         }, )
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);
            }
        },

      async getWait() {
            try {
            const res = await this.$axios.get(`/wait`);
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