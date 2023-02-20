<!-- eslint-disable vue/valid-v-slot -->

<template>
    
    <v-layout row class="justify-center mt-1">
      <v-flex cols="12" sm="12"  md="12" >
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
    <template #item.b_dates="{ item }">
          <span>{{ new Date(item.b_return).toLocaleString('th-TH', { year:'numeric', month:'long',day:'numeric'
           , timeZone: 'UTC' }) }}</span>
    </template>
      <template #item.actions="{ item }">
       
        <v-btn icon  outlined  class="mr-1 teal error"  @click="dedialog = !dedialog ;opendeRe(item.b_id)">
            <v-icon >
        mdi-delete
      </v-icon>
        </v-btn>
    </template>

    

      
    </v-data-table>
    </v-flex>

    <v-dialog
      v-model="dedialog"
      max-width="470"
    >
      <v-card align="center">
       
                
        <!-- Error Success -->
        
        <v-card-title   class="text-h5">
          <v-alert v-show="deerrorAlert"  dense outlined  type="error">
          <v-row align="center">
            ไม่สามารถลบรายการคืนนี้ได้
            <v-spacer/>
            <v-btn text plain @click="deerrorAlert = false">X</v-btn>
          </v-row>
            
        </v-alert>
          ยืนยันที่จะลบรายการคืนนี้หรือไม่
        </v-card-title>

        <v-card-text>
         
        </v-card-text>

        <v-card-actions>

          <v-btn
            color="green darken-1"
            text
            @click="confirmdeRe(de_id)"
          >
            ยืนยัน
          </v-btn>
            <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="dedialog = !dedialog;deerrorAlert = false"
          >
            ยกเลิก
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    </v-layout>
  </template>
   
  
  <script>
//   import SvgIcon from '@jamescoyle/vue-icon';
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
        de:[],
        user:[],
        search: '',
        b_id: '',
        de_id: '',
        deerrorAlert: false,
        dedialog : false,
      }
    },
    
    computed: {
        
      headers () {
        return [
          
        { text: 'ไอดีการคืน', value: 'b_id', filter: value => {
              if (!this.b_id) return true
              return value < parseInt(this.b_id)
            }, },
          { text: 'UserName', value: 'username' },
          { text: 'ชื่ออุปกรณ์', value: 'i_name'  },
          { text: 'รูปภาพ', value: 'img'   },
          { text: 'หมวดหมู่', value: 'c_name' },
          { text: 'จำนวน', value: 'b_qty', sortable: false },
          { text: 'วันที่คืน', value: 'b_dates', sortable: false },
          { text: 'สถานะ', value: 'b_stat', sortable: false },
          { text: 'จัดการ', value: 'actions', sortable: false },
          
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
          this.getmeReturn()
        },
    methods: {
      filterOnlyCapsText (value, search, item) {
        return value != null &&
          search != null &&
          typeof value === 'string' &&
          value.toString().toLocaleUpperCase().includes(search)
      },

      async deReturn(id) {
        
            try {
           const res = await this.$axios.delete(`/borrow/dereturn/${id}`);
           this.de = res.data
           this.dedialog = false
            this.getmeReturn()
            } catch (e) {

            console.error(e);

            }
        },
      async getmeReturn() {
            try {
            const res = await this.$axios.get(`/return/`);
            this.data = res.data;
            // eslint-disable-next-line no-console
            // console.log(this.data);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

        opendeRe(id){
          this.dedialog = true
          this.de_id = id
        },

        confirmdeRe(id){
          // console.log(id)
         this.deReturn(id)
        },

    },
  }
  </script>