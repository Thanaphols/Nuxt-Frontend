<!-- eslint-disable vue/valid-v-slot -->
<template>
    
    <v-row class="justify-center mt-1">
      <v-col cols="12" sm="12"  md="12"  >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" cols="12" sm="12"  md="12" :custom-filter="filterOnlyCapsText" >
   
      <template #top>

        <v-row  align="center">
            <v-col sm="8">
                <v-text-field  v-model="search"  label="Search (UPPER CASE ONLY)" class="mx-5 " ></v-text-field>
            </v-col>
            <v-col sm="4">
            <router-link :to="`/dashboard/addEquipment`" >
                <v-btn  plain text  color="primary" >
                จำนวนผู้ใช้งานทั้งหมด 
                {{numall}}
                </v-btn>
            </router-link>
            </v-col>
        </v-row>
      </template>

     
      
      <template #item.actions="{ item }">
        <router-link :to="`/dashboard/upUser/${ item.u_id }`" >
        <v-btn icon  outlined  class="mr-1 primary">
            <v-icon small   >
        mdi-pencil
      </v-icon>
        </v-btn>
    </router-link>
        <v-btn   icon  outlined  class="mr-1 error" @click="deInv(item.u_id)">
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
   export default {
    middleware: 'auth',
    data () {
      return {
        dialog: false,
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
          
          { text: 'ไอดีผู้ใช้งาน', value: 'u_id', filter: value => {
              if (!this.i_id) return true
              return value < parseInt(this.i_id)
            }, },
            { text: 'อีเมล', value: 'email' },
          { text: 'Username', value: 'username' },
          { text: 'ชื่อ', value: 'firstname'  },
          { text: 'นามสกุล ', value: 'lastname'  },
          { text: 'สถานะ', value: 'u_stat'   },
          
          { text: 'จัดการ', value: 'actions', sortable: false },
          
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
            setTimeout(async () =>{  
          await location.reload()
          //  this.$router.push('/')
         }, )
            
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