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
          <span>{{ new Date(item.b_date).toLocaleString('th-TH', { year:'numeric', month:'long',day:'numeric'
           , timeZone: 'UTC' }) }}</span>
    </template>
      
      <template #item.actions="{ item }">
        <v-btn  icon  outlined  class=" primary"   @click="updialog = !updialog;openupWait(item.b_id,item.b_qty,item.i_id)">
          <svg-icon type="mdi" :path="path2"></svg-icon>
        </v-btn> 
        <v-btn  icon  outlined  class="mr-1 teal error"    @click="dedialog = !dedialog;opendeWait(item.b_id)">
          <v-icon >
        mdi-delete
          </v-icon>
        </v-btn>
    </template>
    </v-data-table>
    </v-flex>

    <v-dialog
      v-model="updialog"
      max-width="470"
    >
      <v-card align="center">
       
                
        <!-- Error Success -->
        
        <v-card-title   class="text-h5">
          <v-alert v-show="uperrorAlert"  dense outlined  type="error">
          <v-row align="center">
            ไม่สามารถอนุมัติได้เพราะไม่มีอุปกรณ์เหลือ
            <v-spacer/>
            <v-btn text plain @click="uperrorAlert = false">X</v-btn>
          </v-row>
            
        </v-alert>
          ยืนยันที่จะอนุมัติคำร้องนี้
        </v-card-title>

        <v-card-text>
         
        </v-card-text>

        <v-card-actions>

          <v-btn
            color="green darken-1"
            text
            @click="confirmdeupWait( up_id,up_qty,up_id2)"
          >
            ยืนยัน
          </v-btn>
            <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="updialog = !updialog;uperrorAlert = false"
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
            ไม่สามารถลบคำร้องขอได้
            <v-spacer/>
            <v-btn text plain @click="deerrorAlert = false">X</v-btn>
          </v-row>
            
        </v-alert>
          ยืนยันที่จะลบคำร้องนี้หรือไม่
        </v-card-title>

        <v-card-text>
         
        </v-card-text>

        <v-card-actions>

          <v-btn
            color="green darken-1"
            text
            @click="confirmdeWait(de_id)"
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
  import { mdiKeyboardReturn,mdiCheckBold  } from '@mdi/js';
   export default {
    name: "AppBorrow",
    components: {
		SvgIcon
	},
    
    middleware: 'auth',
    data () {
      return {
        updialog: false,
        path: mdiKeyboardReturn,
        path2: mdiCheckBold,
        u_id: '',
        data:[],
        up:[],
        delete:[],
        re:[],
        search: '',
        b_id: '',
       up_id : '',
       up_qyu: '',
       up_id2: '',
       uperrorAlert: false,
       deerrorAlert: false,
       dedialog : false,
       de_id: '',
      }
    },
    
    computed: {
        
      headers () {
        return [
          
        { text: 'ไอคำขอยืม', value: 'b_id', filter: value => {
              if (!this.b_id) return true
              return value < parseInt(this.b_id)
            }, },
          { text: 'UserName', value: 'username' },
          { text: 'ชื่ออุปกรณ์', value: 'i_name'  },
          { text: 'รูปภาพ', value: 'img'   },
          { text: 'หมวดหมู่', value: 'c_name' },
          { text: 'จำนวน', value: 'b_qty', sortable: false },
          { text: 'วันที่ยื่นคำร้อง', value: 'b_dates', sortable: false },
          { text: 'จัดการ', value: 'actions', sortable: false },
          
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
        this.uperrorAlert= false
        this.updialog = false
        // eslint-disable-next-line no-console
        // console.log(this.up);
        this.getWait()
        } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
        this.uperrorAlert= true
        }
    },

      async deWait(id) {
        
            try {
            const res = await this.$axios.delete(`/admin/deWait/${id}`);
            this.delete = res.data;
            // eslint-disable-next-line no-console
            // console.log(this.delete);
            this.deerrorAlert= false
            this.dedialog = false
            this.getWait()
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
            // console.log(this.data);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },
        openupWait(id,qty,id2){
          this.updialog = true
          this.up_id = id
          this.up_qty = qty
          this.up_id2 = id2
        },

        confirmdeupWait(id,qty,id2){
          // console.log(id,qty,id2)
         this.upWait(id,qty,id2)
        },

        opendeWait(id){
          this.dedialog = true
          this.de_id = id
        },

        confirmdeWait(id){
          // console.log(id)
         this.deWait(id)
        },
    },
  }
  </script>