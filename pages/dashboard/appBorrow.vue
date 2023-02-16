<!-- eslint-disable vue/valid-v-slot -->
<template>
    <v-row class="justify-center mt-1">
      <v-col cols="12" sm="12"  md="10" >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
   
      <template #top>
        <v-text-field
          v-model="search"
          label="Search (UPPER CASE ONLY)"
          class="mx-4 "
        ></v-text-field>
      </template>
      
      <template #item.actions="{ item }">
        <div class="form-inline">
        <v-btn color="primary" class="mr-4"   @click="upWait(item.b_id,item.b_qty,item.i_id)">
            Approve
        </v-btn> <br>
        
        <v-btn color="error"    @click="deWait(item.b_id)">
            Delete
        </v-btn>
    </div>
    </template>

    

      
    </v-data-table>
    </v-col>
    </v-row>
    
  </template>
   
  
  <script>
//   import SvgIcon from '@jamescoyle/vue-icon';
  import { mdiKeyboardReturn } from '@mdi/js';
   export default {
    name: "AppBorrow",
    components: {
		// SvgIcon
	},
    
    middleware: 'auth',
    data () {
      return {
        path: mdiKeyboardReturn,
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
          { text: 'User (id)', value: 'u_id' },
          { text: 'Inventory (id)', value: 'i_id'  },
          { text: 'Borrow Date', value: 'b_date'   },
          { text: 'Status', value: 'b_stat' },
          { text: 'Quetity', value: 'b_qty', sortable: false },
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
        this.$router.go('/dashboard/appBorrow/')
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
            this.$router.go('/dashboard/appBorrow/')
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