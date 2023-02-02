<!-- eslint-disable vue/valid-v-slot -->
<template>
    
    <v-row justify="center mt-1">
      <v-col cols="12" sm="12"  md="10" >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
   
      <template #top>

        <v-row  align="center">
            <v-col sm="10">
                <v-text-field  v-model="search"  label="Search (UPPER CASE ONLY)" class="mx-5 " ></v-text-field>
            </v-col>
            <v-col sm="2">
            <router-link :to="`/inv/addEquipment`" >
                <v-btn    class=""  depressed  color="primary" >
                <v-text  >  เพิ่มสินค้า </v-text>
                </v-btn>
            </router-link>
            </v-col>
        </v-row>
      </template>

     
      
      <template #item.actions="{ item }">
        <router-link :to="`/inv/${ item.i_id }`" >
        <button icon  class="mr-1"  >
            
            <v-icon small   >
        mdi-pencil
      </v-icon>
        </button>
    </router-link>
        <button icon  class="mr-1"  @click="deInv(item.i_id)">
            <v-icon >
        mdi-delete
      </v-icon>
        </button>
      
    </template>

      
    </v-data-table>
    </v-col>
    </v-row>
    
  </template>
   
  
  <script>
   export default {
    middleware: 'auth',
    data () {
      return {
        u_id: '',
        data:[],
        search: '',
        i_id: '',
       
      }
    },
    
    computed: {
        
      headers () {
        return [
          
          { text: 'Equipment (id)', value: 'i_id', filter: value => {
              if (!this.i_id) return true
              return value < parseInt(this.i_id)
            }, },
          { text: 'Equipment Name', value: 'i_name' },
          { text: 'Quatity (id)', value: 'i_qty'  },
          { text: 'Category ', value: 'i_category'  },
          { text: 'Status', value: 'i_stat'   },
          { text: 'Image', value: 'i_img' },
          { text: 'Actions', value: 'actions', sortable: false },
          
        ]
      },
    },
    mounted() {
          // eslint-disable-next-line no-console
          // console.log(this.desserts)
          this.user = this.$auth.user
           this. u_id = this.user.u_id
           // eslint-disable-next-line no-console
          console.log(this.u_id)
          this.getEq()
        },
    methods: {
      filterOnlyCapsText (value, search, item) {
        return value != null &&
          search != null &&
          typeof value === 'string' &&
          value.toString().toLocaleUpperCase().includes(search)
      },

      async deInv(id) {
       
            try {
            const res = await this.$axios.delete(`/inv/deInv/${id}`);
            this.data = res.data;
            // eslint-disable-next-line no-console
            console.log(this.data);
            this.$router.push('/inv/equipment/')
            
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },
    
        

      async getEq() {
            try {
            const res = await this.$axios.get(`/inv/`);
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