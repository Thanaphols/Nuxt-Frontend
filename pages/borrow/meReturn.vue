<!-- eslint-disable vue/valid-v-slot -->

<template>
    
    <v-row justify="center mt-1">
      <v-col cols="12" sm="12"  md="10" >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
   
      <template #top>
        <v-text-field
          v-model="search"
          label="Search (UPPER CASE ONLY)"
          class="mx-4 "
        ></v-text-field>
      </template>
      
      <!-- <template #item.actions="{ item }">
        <button icon  class="mr-1"  @click="upBorrow(item.b_id,item.b_qty,item.i_id)">
            <v-icon small   >
        mdi-pencil
      </v-icon>
        </button>
        <button icon  class="mr-1"  @click="deBorrow(item.b_id,item.b_qty,item.i_id)">
            <v-icon >
        mdi-delete
      </v-icon>
        </button>
    </template> -->


      
    </v-data-table>
    </v-col>
    </v-row>
    
  </template>
   
  
  <script>
  // import SvgIcon from '@jamescoyle/vue-icon';
  import { mdiKeyboardReturn } from '@mdi/js';
   export default {
    name: "MyCoolComponent",
    components: {
		// SvgIcon
	},
    
    middleware: 'auth',
    data () {
      return {
        path: mdiKeyboardReturn,
        u_id: '',
        data:[],
        user:[],
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
          // { text: 'Actions', value: 'actions', sortable: false },
          
        ]
      },
    },
    mounted() {
          
          // console.log(this.showAlert)
          // console.log(this.errorAlert)
          // eslint-disable-next-line no-console
          // console.log(this.desserts)
          this.user = this.$auth.user
           this. u_id = this.user.u_id
           // eslint-disable-next-line no-console
          console.log(this.u_id)
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
            this.data = res.data;
            // eslint-disable-next-line no-console
            console.log(this.data);
            this.$router.push('/borrow/meBorrow/')
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

        async upBorrow(id,qty,id2) {
        
        try {
        const res = await this.$axios.delete(`/borrow/deBorrow/${id}/${qty}/${id2}`);
        this.data = res.data;
        // eslint-disable-next-line no-console
        console.log(this.data);
        this.$router.push('/borrow/meBorrow/')
        } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);

        }
    },

    async reBorrow(id,qty,id2) {
        
        try {
        const res = await this.$axios.patch(`/borrow/return/${id}/${qty}/${id2}`);
        this.data = res.data;
        // eslint-disable-next-line no-console
        console.log(this.data);
        this.$router.push('/borrow/meBorrow/')
        } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);

        }
    },
    
        

      async getmeBorrow() {
            try {
            const res = await this.$axios.get(`/borrow/return/${this.u_id}`);
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