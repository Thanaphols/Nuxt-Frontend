<template>
  <!-- <v-container  >
    
    <v-row>
      <v-col sm="4">
        <v-card class="red lighten-1">
      <v-card-title>
        Number of Equipment {{ n }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
      <v-col sm="4">
        <v-card class="pink lighten-1">
      <v-card-title>
        Number of borrowed items {{ n2 }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
      <v-col sm="4">
        <v-card class="purple lighten-1" >
      <v-card-title>
        number of returned items {{ n3 }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
    
    
      <v-col sm="4">
        <v-card class="deep-purple lighten-1" >
      <v-card-title>
        Number of Request Borrow {{ n4 }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
      <v-col sm="4">
        <v-card class="indigo lighten-1" >
      <v-card-title>
        Number of User {{ n5 }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
      <v-col sm="4">
        <v-card class="teal lighten-1" >
      <v-card-title>
        Number of User NotApprove {{ n6 }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
      <v-col sm="4">
        <v-card class="blue lighten-1" >
      <v-card-title>
        Number of User Approve {{ n7 }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
      <v-col sm="4">
        <v-card class="light-blue lighten-1" >
      <v-card-title>
        Number of User Admin {{ n8 }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
      <v-col sm="4">
        <v-card class="cyan lighten-1" >
      <v-card-title>
        Number of Category {{ n9 }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
    </v-row>
  </v-container> -->
  
  <div class="team">
  <v-container >
      <v-layout row wrap>
          <v-flex col3 md3 sm3 xs4>
              <h1 class="subheading " dark>Dashboard</h1>
          </v-flex>
      </v-layout>
    <v-layout row wrap>
       <v-flex sm6 xs12  md6  lg4 >
    
     <v-card class="ma-3" warp light elevation="11" outlined>
  <v-list-item  >
     <v-list-item-avatar tile class="mt-n7"  >
      <v-btn icon class="teal  lighten-2"><svg-icon type="mdi" :path="path"></svg-icon></v-btn>
    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">จำนวนคำร้องขอยืมอุปกรณ์ทั้งหมด</div>
      <v-list-item-title class="headline mb-1 text-right" >{{ n1 }}</v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
      <v-btn  color="teal  lighten-2" text @click="getWait();" >ดูรายการคำร้องขอทั้งหมด</v-btn>
  </v-card-actions>
</v-card>
    </v-flex>
    <v-flex sm6 xs12  md6  lg4 >
    
     <v-card class="ma-3" warp light elevation="11" outlined>
  <v-list-item  >
     <v-list-item-avatar tile class="mt-n7"  >
      <v-btn icon class="lime lighten-1"><svg-icon type="mdi" :path="path2"></svg-icon></v-btn>
      

    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">จำนวนการยืมอุปกรณ์ทั้งหมด</div>
      <v-list-item-title class="headline mb-1 text-right" >{{ n2 }}</v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
      <v-btn  color="teal  lighten-2" text @click="getBor()" >ดูรายการยืมทั้งหมด</v-btn>
  </v-card-actions>
</v-card>
    </v-flex>
    <v-flex sm6 xs12  md6  lg4
    >
    
     <v-card class="ma-3"  warp light elevation="11" outlined>
  <v-list-item>
     <v-list-item-avatar tile class="mt-n7"  >
      <v-btn icon class="success"><svg-icon type="mdi" :path="path3"></svg-icon></v-btn>
      
    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">จำนวนการคืนอุปกรณ์ทั้งหมด</div>
      <v-list-item-title class="headline mb-1 text-right" >{{ n3 }}</v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
      <v-btn  color="teal  lighten-2" text @click="getReturn()" >ดูรายการคืนทั้งหมด</v-btn>
  </v-card-actions>
</v-card>
    </v-flex>
     
     
    </v-layout>

    <v-layout v-show="wait" row >
          <v-flex col12 md12 sm12 xs12>
              <v-data-table :headers="headers" :items="data"  class="elevation-1 justtify-center" :search="search" :custom-filter="filterOnlyCapsText" >
              <template #top>
                  <v-text-field
                  v-model="search"
                  label="Search (UPPER CASE ONLY)"
                  class="mx-4 "
                  ></v-text-field>
              </template>
              <template #[`item.date`]="{ item }">
                  
                      <span>{{ new Date(item.b_date).toLocaleString('th-TH', { year:'numeric', month:'long',day:'numeric', timeZone: 'UTC'
                                          }) }}</span>
                      
              </template>
              <template #[`item.img`]="{ item }">
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
              </v-data-table>
          </v-flex>
    </v-layout>
    <v-layout v-show="bor" row >
          <v-flex col12 md12 sm12 xs12>
              <v-data-table :headers="headers2" :items="borrow"  class="elevation-1 justtify-center" :search="search" :custom-filter="filterOnlyCapsText" >
              <template #top>
                  <v-text-field
                  v-model="search"
                  label="Search (UPPER CASE ONLY)"
                  class="mx-4 "
                  ></v-text-field>
              </template>
              <template #[`item.date`]="{ item }">
                  
                      <span>{{ new Date(item.b_date).toLocaleString('th-TH', { year:'numeric', month:'long',day:'numeric', timeZone: 'UTC'
                                          }) }}</span>
                      
              </template>
              <template #[`item.img`]="{ item }">
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
              </v-data-table>
          </v-flex>
    </v-layout>
    <v-layout v-show="re" row wrap>
          <v-flex col12 md12 sm12 xs12>
              <v-data-table :headers="headers3" :items="returns"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
              <template #top>
                  <v-text-field
                  v-model="search"
                  label="Search (UPPER CASE ONLY)"
                  class="mx-4 "
                  ></v-text-field>
              </template>
              <template #[`item.date`]="{ item }">
                  <span>{{ new Date(item.b_date).toLocaleString('th-TH', { year:'numeric', month:'long',day:'numeric'
                  , timeZone: 'UTC' }) }}</span>
              </template>
              <template #[`item.date2`]="{ item }">
                  <span>{{ new Date(item.b_return).toLocaleString('th-TH', { year:'numeric', month:'long',day:'numeric'
                  , timeZone: 'UTC' }) }}</span>
              </template>
          <template #[`item.img`]="{ item }">
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
              </v-data-table>
          </v-flex>
    </v-layout> 
  </v-container>
</div>

</template>

<script>
  import SvgIcon from '@jamescoyle/vue-icon';
  import { mdiCheckboxMultipleMarkedCircleOutline,mdiKeyboardReturn,mdiCheckboxMarkedCircleOutline  } from '@mdi/js';
  
export default {
  

  name: "HomeView",
  
  components:{
      // eslint-disable-next-line vue/no-unused-components
      SvgIcon
  },
  middleware: 'auth',
  data() {
  return {
      data:[],
      borrow:[],
      returns:[],
      wait: false,
      bor: false,
      re: false,
      path: mdiCheckboxMultipleMarkedCircleOutline ,
      path2: mdiCheckboxMarkedCircleOutline   ,
      path3: mdiKeyboardReturn  ,
  user:{
      u_id: '',
      email: '',
      username: '',
      firstname: '',
      lastname: '',
      u_stat: '',
      },
      search: '',
    n1: 0,
    n2: 0,
    n3: 0,
    team: [
      
  ]

      };
  },
  computed:{
      headers () {
      return [
        
      { text: 'ไอดีคำขอยืม', value: 'b_id', filter: value => {
            if (!this.b_id) return true
            return value < parseInt(this.b_id)
          }, width: '120px'  },
        { text: 'UserName', value: 'username',width: '120px'   },
        { text: 'ชื่ออุปกรณ์', value: 'i_name' ,width: '120px'   },
        { text: 'รูปอุปกรณ์', value: 'img' ,width: '120px' ,sortable: false  },
        { text: 'หมวดหมู่', value: 'c_name',width: '120px'  },
        { text: 'จำนวนที่ขอยืม', value: 'b_qty',  width: '130px'   },
        { text: 'วันที่ขอยืม', value: 'date', width: '150px'  },
        { text: 'สถานะ', value: 'b_stat', sortable: false ,width: '100px'   },
        
      ]
    },
    headers2 () {
      return [
        
      { text: 'ไอดีการยืม', value: 'b_id', filter: value => {
            if (!this.b_id) return true
            return value < parseInt(this.b_id)
          },  width: '120px' },
        { text: 'UserName', value: 'username' ,width: '120px'  },
        { text: 'ชื่ออุปกรณ์', value: 'i_name', width: '120px' },
        { text: 'รูปอุปกรณ์', value: 'img',width: '120px' ,sortable: false    },
        { text: 'หมวดหมู่', value: 'c_name',width: '120px' },
        { text: 'จำนวนที่ยืม', value: 'b_qty', sortable: false,  width: '120px' },
        { text: 'วันที่ยืม', value: 'date', sortable: false , width: '150px' },
        { text: 'สถานะ', value: 'b_stat', sortable: false ,width: '100px' },
        
      ]
    },
    headers3 () {
      return [
        
      { text: 'ไอดีการคืน', value: 'b_id', filter: value => {
            if (!this.b_id) return true
            return value < parseInt(this.b_id)
          }, width: '110px'},
        { text: 'UserName', value: 'username',width: '110px' },
        { text: 'ชื่ออุปกรณ์', value: 'i_name' , width: '110px' },
        { text: 'รูปอุปกรณ์', value: 'img' ,width: '80px' ,sortable: false   },
        { text: 'หมวดหมู่', value: 'c_name',width: '100px'  },
        { text: 'จำนวนที่คืน', value: 'b_qty', sortable: false, width: '100px' },
        { text: 'วันที่ขอยืม', value: 'date', sortable: false , width: '120px' },
        { text: 'วันที่คืน', value: 'date2', sortable: false , width: '120px' },
        { text: 'สถานะ', value: 'b_stat', sortable: false ,width: '60px' },
        
      ]
    },
  },
  created() {
  this.user = this.$auth.user
  this.getwa()
  this.getbor()
  this.getre()
  },
  methods: {
      filterOnlyCapsText (value, search, item) {
      return value != null &&
        search != null &&
        typeof value === 'string' &&
        value.toString().toLocaleUpperCase().includes(search)
    },
      // number
  async getwa() {
    try {
      const id = this.user.u_id;
      const res = await this.$axios.get(`/borrow/wait/${id}`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
      this.n1 = res.data.length;
      this.load = false;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getbor() {
    try {
      const id = this.user.u_id;
      const res = await this.$axios.get(`/borrow/borrow/${id}`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
      this.n2 = res.data.length;
      this.load = false;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getre() {
    try {
      const id = this.user.u_id;
      const res = await this.$axios.get(`/borrow/return/${id}`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
      this.n3 = res.data.length;
      this.load = false;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  // table data
  async getReturn() {
      try {
          this.wait = false
          this.bor = false
          this.re = !this.re
          const id = this.user.u_id;
          const res = await this.$axios.get(`/borrow/return/${id}`);
          this.returns = res.data;
          } catch (e) {
          // eslint-disable-next-line no-console
          console.error(e);
          }
  },
  async getWait() {
      try {
          this.wait = !this.wait
          this.bor = false
          this.re = false
          const id = this.user.u_id;
          const res = await this.$axios.get(`/borrow/wait/${id}`);
          this.data = res.data;
          } catch (e) {
          // eslint-disable-next-line no-console
          console.error(e);
          }
      },
  async getBor() {
      try {
          this.wait = false
          this.bor = !this.bor
          this.re = false
          const id = this.user.u_id;
          const res = await this.$axios.get(`/borrow/borrow/${id}`);
          this.borrow = res.data;
          } catch (e) {
          // eslint-disable-next-line no-console
          console.error(e);
          }
  },
  
},
}

</script>
