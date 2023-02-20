<!-- eslint-disable vue/valid-v-slot -->
<template>
    <v-layout row class="justify-center mt-1">
      <v-flex cols="12" sm="12"  md="12"  >
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
       circle
      :src="require(`~/assets/images/noimg.png`)"
    ></v-img>
    <v-img
      v-else
      height="100"
      width="100"
       circle
      :src="require(`~/assets/images/${item.i_img}`)"
    ></v-img>
        
    </template>
      
      <template #item.actions="{ item }">
        <!-- <button icon  class="mr-1"  @click="upBorrow(item.b_id,item.b_qty,item.i_id)">
            <v-icon small   >
        mdi-pencil
      </v-icon>
        </button> -->
        <v-btn icon  outlined  class="mr-1 primary"  @click="dedialog= !dedialog;openDe(item.b_id,item.b_qty,item.i_id)">
            <v-icon >
        mdi-delete
      </v-icon>
        </v-btn>
    </template>
    <template #item.b_dates="{ item }">
          <span>{{ new Date(item.b_date).toLocaleString('th-TH', { year:'numeric', month:'long',day:'numeric'
           , timeZone: 'UTC' }) }}</span>
    </template>
    <template #item.Return="{ item }">
        <v-btn icon  outlined  class="mr-1 teal lighten-1"  @click="redialog= !redialog ;openRe(item.b_id,item.b_qty,item.i_id)">
          <svg-icon type="mdi" :path="path"></svg-icon>
        </v-btn>
       
    </template>

      
    </v-data-table>
    </v-flex>

    <v-dialog
      v-model="redialog"
      max-width="470"
    >
      <v-card align="center">
       
                
        <!-- Error Success -->
        
        <v-card-title   class="text-h5">
          <v-alert v-show="reerrorAlert"  dense outlined  type="error">
          <v-row align="center">
            ไม่สามารถคืนรายการคืนนี้ได้
            <v-spacer/>
            <v-btn text plain @click="reerrorAlert = false">X</v-btn>
          </v-row>
            
        </v-alert>
          ยืนยันที่จะคืนรายการคืนนี้หรือไม่
        </v-card-title>

        <v-card-text>
         
        </v-card-text>

        <v-card-actions>

          <v-btn
            color="green darken-1"
            text
            @click="confirmRe(r_id,r_qty,r_id2)"
          >
            ยืนยัน
          </v-btn>
            <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="redialog = !redialog;reerrorAlert = false"
          >
            ยกเลิก
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

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
            @click="confirmDe(d_id,d_qty,d_id2)"
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
  import SvgIcon from '@jamescoyle/vue-icon';
  import { mdiKeyboardReturn } from '@mdi/js';
   export default {
    name: "MeBorrow",
    components: {
		SvgIcon
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
        b_id: '',
        r_id: '',
        r_qty: '',
        r_id2: '',
        reerrorAlert: false,
        redialog: false,
        dedialog: false,
        deerrorAlert: false,
        d_id: '',
        d_qty: '',
        d_id2: '',
      }
    },
    
    computed: {
        
      headers () {
        return [
          
        { text: 'ไอดีการยืม', value: 'b_id', filter: value => {
              if (!this.b_id) return true
              return value < parseInt(this.b_id)
            }, },
          { text: 'UserName', value: 'username' },
          { text: 'ชื่ออุปกรณ์', value: 'i_name'  },
          { text: 'รูปภาพ', value: 'img'   },
          { text: 'หมวดหมู่', value: 'c_name' },
          { text: 'จำนวน', value: 'b_qty', sortable: false },
          { text: 'วันที่ยืม', value: 'b_dates', sortable: false },
          { text: 'สถานะ', value: 'b_stat', sortable: false },
          { text: 'จัดการ', value: 'actions', sortable: false },
          { text: 'คืนอุปกรณ์', value: 'Return', sortable: false },
          
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
          // console.log(this.u_id)
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
            // eslint-disable-next-line no-console
            // console.log(this.delete);
            this.dedialog = false 
            this.getmeBorrow()
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);
            this.dedialog = true
            }
        },

        

    async reBorrow(id,qty,id2) {
        
        try {
        const res = await this.$axios.patch(`/borrow/return/${id}/${qty}/${id2}`);
        this.re = res.data;
        // eslint-disable-next-line no-console
        this.redialog = false 
        this.getmeBorrow()
        } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
        this.redialog = true
        }
    },
    
        

      async getmeBorrow() {
            try {
            const res = await this.$axios.get(`/borrow`);
            this.data = res.data;
            // eslint-disable-next-line no-console
            // console.log(this.data);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

        openDe(id,qty,id2){
          this.dedialog = true
          this.d_id = id
          this.d_qty = qty
          this.d_id2 = id2
          // console.log(id,qty,id2)
        },

        confirmDe(id,qty,id2){
          // console.log()
        this.deBorrow(id,qty,id2)
        },

        openRe(id,qty,id2){
          this.redialog = true
          this.r_id = id
          this.r_qty = qty
          this.r_id2 = id2

        },

        confirmRe(id,qty,id2){
        this.reBorrow(id,qty,id2)
        },

    },
  }
  </script>

  <style></style>