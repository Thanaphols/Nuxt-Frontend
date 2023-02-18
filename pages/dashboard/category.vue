<!-- eslint-disable vue/valid-v-slot -->
<template>
    <v-row class="justify-center mt-1">
      <v-col cols="12" sm="12"  md="12"  >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
   
      <template #top>
        <v-row  align="center">
        <v-col sm="7">
        <v-text-field
          v-model="search"
          label="Search (UPPER CASE ONLY)"
          class="mx-4 "
        ></v-text-field>
      </v-col>
        <v-col sm="3">
                <div  color="primary" >  จำนวนหมวดหมู่ทั้งหมด {{ numall }} </div>
            </v-col>
            <v-col sm="2">
            <router-link :to="`/dashboard/addcategory`" >
                <v-btn    class=""   depressed  color="primary" >
                <v-text  >  เพิ่มหมวดหมู่ </v-text>
                </v-btn>
            </router-link>
            </v-col>
          </v-row>
      </template>
      
      <template #item.actions="{ item }">
        <router-link :to="`/dashboard/upCate/${ item.c_id }`" >
        <v-btn icon  outlined  class="mr-1 teal primary"  >
            
            <v-icon small   >
        mdi-pencil
      </v-icon>
        </v-btn>
    </router-link>
        <v-btn icon  outlined  class="mr-1 teal error"  @click="deCate(item.c_id)">
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
  import { mdiKeyboardReturn } from '@mdi/js';
   export default {
    name: "CateGory",
    components: {
	},
    
    middleware: 'auth',
    data () {
      return {
        path: mdiKeyboardReturn,
        u_id: '',
        data:[],
        up:[],
        delete:[],
        user:[],
        re:[],
        search: '',
        numall:  '',
      }
    },
    
    computed: {
        
      headers () {
        return [
          
          { text: 'Categry (id)', value: 'c_id', filter: value => {
              if (!this.b_id) return true
              return value < parseInt(this.b_id)
            }, },
          { text: 'Category Name ', value: 'c_name' },
          { text: 'Actions', value: 'actions', sortable: false },
          
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
          this.getmecate()
        },
    methods: {
      filterOnlyCapsText (value, search, item) {
        return value != null &&
          search != null &&
          typeof value === 'string' &&
          value.toString().toLocaleUpperCase().includes(search)
      },

      async deCate(id) {
        
            try {
            const res = await this.$axios.delete(`/admin/deCate/${id}/`);
            this.delete = res.data;
            // eslint-disable-next-line no-console
           
            setTimeout(async () =>{  
          await location.reload()
          //  this.$router.push('/')
         }, )
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

        

    async reBorrow(id,qty,id2) {
        
        try {
        const res = await this.$axios.patch(`/borrow/return/${id}/${qty}/${id2}`);
        this.re = res.data;
        // eslint-disable-next-line no-console
        console.log(this.re);
        this.$router.push('/borrow/meBorrow/')
        } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);

        }
    },
    
        

      async getmecate() {
            try {
            const res = await this.$axios.get(`/cate/`);
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