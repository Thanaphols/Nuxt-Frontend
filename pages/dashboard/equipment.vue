<!-- eslint-disable vue/valid-v-slot -->
<template>
    
    <v-row justify="center mt-1">
      <v-col cols="12" sm="12" mt="2"  md="10" >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
   
      <template #top>

        <v-row  align="center">
            <v-col sm="8">
                <v-text-field  v-model="search"  label="Search (UPPER CASE ONLY)" class="mx-5 " ></v-text-field>
            </v-col>
            <v-col sm="2">
                <div  color="primary" >  จำนวนสินค้าทั้งหมด {{ numall }} </div>
            </v-col>
            <v-col sm="2">
            <router-link :to="`/dashboard/addequipment`" >
                <v-btn    class=""   depressed  color="primary" >
                <v-text  >  เพิ่มสินค้า </v-text>
                </v-btn>
            </router-link>
            </v-col>
        </v-row>
        {{ search }}
      </template>

      <template #item.category="{ item }">
        
        <div v-for=" ca in cate " :key="ca.c_id" >
          
       {{  item.i_category == ca.c_id ? ca.c_name : ''}}
    </div>
    </template>
     
      
      <template #item.actions="{ item }">
        <router-link :to="`/dashboard/upEq/${ item.i_id }`" >
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
       numall:'',
       cate:[],
       de:[],
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
          { text: 'Quatity', value: 'i_qty'  },
          { text: 'Category ', value: 'category'  },
          { text: 'Status', value: 'i_stat'   },
        //   { text: 'Image', value: 'i_img' },
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
          this.getcate()
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
            this.de = res.data;
            // eslint-disable-next-line no-console
            console.log(this.de);
            this.$router.go('/dashboard/equipment/')
            
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },
    
        

      async getEq() {
            try {
            const res = await this.$axios.get(`/inv/`);
            this.data = res.data;
            this.numall = this.data.length;
            // eslint-disable-next-line no-console
            console.log(this.data);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

        async getcate() {
            try {
            const res = await this.$axios.get(`/cate/`);
            this.cate = res.data;
            // eslint-disable-next-line no-console
            console.log(this.cate);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

    },
  }
  </script>