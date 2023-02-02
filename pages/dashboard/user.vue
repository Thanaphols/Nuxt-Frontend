<!-- eslint-disable vue/valid-v-slot -->
<template>
    
    <v-row justify="center mt-1">
      <v-col cols="12" sm="12"  md="10" >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
   
      <template #top>

        <v-row  align="center">
            <v-col sm="8">
                <v-text-field  v-model="search"  label="Search (UPPER CASE ONLY)" class="mx-5 " ></v-text-field>
            </v-col>
            <v-col sm="4">
            <router-link :to="`/dashboard/addEquipment`" >
                <v-btn disabled    class=""  depressed  color="primary" >
                <v-text  >จำนวนผู้ใช้งานทั้งหมด </v-text>
                <v-text  > {{numall}}</v-text>
                </v-btn>
            </router-link>
            </v-col>
        </v-row>
      </template>

     
      
      <template #item.actions="{ item }">
        <router-link :to="`/dashboard/upUser/${ item.u_id }`" >
        <button icon  class="mr-1"  >
            
            <v-icon small   >
        mdi-pencil
      </v-icon>
        </button>
    </router-link>
        <button icon  class="mr-1"  @click="deInv(item.u_id)">
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
        delete:[],
        search: '',
        i_id: '',
        numall:'',
        
       
      }
    },
    
    computed: {
        
      headers () {
        return [
          
          { text: 'User (id)', value: 'u_id', filter: value => {
              if (!this.i_id) return true
              return value < parseInt(this.i_id)
            }, },
            { text: 'Email', value: 'email' },
          { text: 'Username', value: 'username' },
          { text: 'Firstname', value: 'firstname'  },
          { text: 'Lastname ', value: 'lastname'  },
          { text: 'User Status', value: 'u_stat'   },
          
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
            const res = await this.$axios.delete(`/users/deUser/${id}`);
            this.delete = res.data;
            // eslint-disable-next-line no-console
            console.log(this.delete);
            this.$router.push('/borrow/meBorrow/')
            
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

    
        

      async getEq() {
            try {
            const res = await this.$axios.get(`/users/`);
            this.data = res.data;
            this.numall = res.data.length;
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