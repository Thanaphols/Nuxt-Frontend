<template>
  <div class="team">
  <v-container >
    <!-- <v-layout class="  blue lighten-3" row wrap> -->
    <v-layout row wrap>
       <v-flex sm6 xs12  md6  lg4 >
     <v-card class="ma-3 blue darken-4" warp dark elevation="11" outlined>
  <v-list-item  >
     <v-list-item-avatar tile class="mt-n7"  >
      <v-btn icon class="teal  lighten-2"><svg-icon type="mdi" :path="path"></svg-icon></v-btn>
    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">จำนวนอุปกรณ์ทั้งหมดในระบบ</div>
      <v-list-item-title class="headline mb-1 text-right" > {{ n1 }}</v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
  </v-card-actions>
</v-card>
    </v-flex>
    <v-flex sm6 xs12  md6  lg4 >
     <v-card class="ma-3 blue darken-3" warp dark elevation="11" outlined>
  <v-list-item  >
     <v-list-item-avatar tile class="mt-n7"  >
      <v-btn icon class="amber darken-3"><svg-icon type="mdi" :path="path2"></svg-icon></v-btn>
    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">จำนวนอุปกรณ์ทั้งหมดที่กำลังถูกยืม</div>
      <v-list-item-title class="headline mb-1 text-right" >{{ n2 }}</v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
  </v-card-actions>
</v-card>
    </v-flex>
    <v-flex sm6 xs12  md6  lg4 >
     <v-card class="ma-3 blue darken-2"  warp dark elevation="11" outlined>
  <v-list-item>
     <v-list-item-avatar tile class="mt-n7"   >
      <v-btn icon class="pink darken-1"><svg-icon type="mdi" :path="path3"></svg-icon></v-btn>
    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">จำนวนหมวดหมู่ทั้งหมด</div>
      <v-list-item-title class="headline mb-1 text-right" >{{ n3 }}</v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
  </v-card-actions>
</v-card>
    </v-flex>
    </v-layout>
    <v-layout row no-gutters  >
        <v-flex class="pa-2" col4 md4 sm4 xs12 outlined  tile >
                  <v-card class="ma-3 pink darken-4" warp dark elevation="11" outlined>
                    <v-list-item  >
                      <v-list-item-avatar tile class="mt-n7"  >
                        <v-btn icon class="indigo darken-4"><svg-icon type="mdi" :path="path4"></svg-icon></v-btn>
                      </v-list-item-avatar>
                      <v-list-item-content>
                        <div class="overline text-left">จำนวนรายการยืม-คืนทั้งหมด</div>
                        <v-list-item-title class="headline mb-1 text-right" > {{ n4 }}</v-list-item-title>
                        <div><v-divider></v-divider></div>
                      </v-list-item-content> 
                    </v-list-item>
                    <v-card-actions>

                    </v-card-actions>
                  </v-card>
                  <v-card class="ma-3 pink darken-3" warp dark elevation="11" outlined>
                    <v-list-item  >
                      <v-list-item-avatar tile class="mt-n7"  >
                        <v-btn icon class="teal  deep-orange lighten-1"><svg-icon type="mdi" :path="path5"></svg-icon></v-btn>
                      </v-list-item-avatar>
                      <v-list-item-content>
                        <div class="overline text-left">จำนวนรายการคำร้องขอทั้งหมด</div>
                        <v-list-item-title class="headline mb-1 text-right" > {{ n5 }}</v-list-item-title>
                        <div><v-divider></v-divider></div>
                      </v-list-item-content> 
                    </v-list-item>
                    <v-card-actions>
                    </v-card-actions>
                  </v-card>
        </v-flex>
        <v-flex  class="pa-2" col4 md4 sm4 xs12 outlined tile >
          <v-card class="ma-3 mt-3 light-green darken-3" height="230px"  dark elevation="11" outlined>
                    <v-list-item  >
                      <v-list-item-avatar tile class="mt-n7"  >
                        <v-btn icon class="teal  orange darken-4"><svg-icon type="mdi" :path="path6"></svg-icon></v-btn>
                      </v-list-item-avatar>
                      <v-list-item-content>
                        <v-list-item-title class="headline mb-1 text-right" > 
                            <v-list-item-content>
                              <v-autocomplete
                            v-model="category" :items="cate" item-value="c_id"  item-text="c_name" density="compact" style="width:200px"   
                            label="ค้นหาจำนวนอุปกรณ์ตามหมวดหมู่" @change="getnumcate(category)" >
                            </v-autocomplete>
                            <v-list-item-title class="headline mb-1 text-right" > 
                          <v-list-item-content>
                              <div class="overline text">จำนวนอุปกรณ์  </div>
                            <v-list-item-title class="headline mb-1 text-right" > {{ n6 }}</v-list-item-title>
                          </v-list-item-content> 
                      </v-list-item-title>
                      </v-list-item-content> 
                        </v-list-item-title>
                       
                        <div><v-divider></v-divider></div>
                      </v-list-item-content> 
                    </v-list-item>
                   <v-card-action>
                    
                   </v-card-action>
                  </v-card>
        </v-flex>
        <v-flex  class="pa-2" col4 md4 sm4 xs12 outlined tile >
          <v-card class="ma-3 mt-3 deep-orange darken-3" height="230px"  dark elevation="11" outlined>
                    <v-list-item  >
                      <v-list-item-avatar tile class="mt-n7"  >
                        <v-btn icon class="teal  green darken-4"><svg-icon type="mdi" :path="path7"></svg-icon></v-btn>
                      </v-list-item-avatar>
                      <v-list-item-content>
                        <v-list-item-title class="headline mb-1 text-right" > 
                            <v-list-item-content>
                              <v-autocomplete
                            v-model="equip" :items="Eq" item-value="i_id"  item-text="i_name" density="compact" style="width:200px"   
                            label="ค้นหาจำนวนอุปกรณ์" @change="getnumeq(equip)" >
                            </v-autocomplete>
                            <v-list-item-title class="headline mb-1 text-right" > 
                          <v-list-item-content>
                              <div class="overline text">จำนวนอุปกรณ์  </div>
                            <v-list-item-title class="headline mb-1 text-right" > {{ n7 }}</v-list-item-title>
                          </v-list-item-content> 
                      </v-list-item-title>
                      </v-list-item-content> 
                        </v-list-item-title>
                       
                        <div><v-divider></v-divider></div>
                      </v-list-item-content> 
                    </v-list-item>
                   <v-card-action>
                    
                   </v-card-action>
                  </v-card>
        </v-flex>
    </v-layout>
    <v-layout row wrap>
       <v-flex sm6 xs12  md6  lg4 >
     <v-card class="ma-3 teal darken-1" warp dark elevation="11" outlined>
  <v-list-item  >
     <v-list-item-avatar tile class="mt-n7"  >
      <v-btn icon class="teal  primary"><svg-icon type="mdi" :path="path9"></svg-icon></v-btn>
    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">จำนวนผู้ใช้งานทั้งหมดในระบบ</div>
      <v-list-item-title class="headline mb-1 text-right" > {{ n9 }} </v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
  </v-card-actions>
</v-card>
    </v-flex>
    <v-flex sm6 xs12  md6  lg4 >
     <v-card class="ma-3 teal darken-2" warp dark elevation="11" outlined>
  <v-list-item  >
     <v-list-item-avatar tile class="mt-n7"  >
      <v-btn icon class="amber darken-3"><svg-icon type="mdi" :path="path10"></svg-icon></v-btn>
    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">ผู้ใช้งานที่ยังไม่ได้รับการอนุมัติ</div>
      <v-list-item-title class="headline mb-1 text-right" >{{ n10 }}</v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
  </v-card-actions>
</v-card>
    </v-flex>
    <v-flex sm6 xs12  md6  lg4 >
     <v-card class="ma-3 teal darken-3"  warp dark elevation="11" outlined>
  <v-list-item>
     <v-list-item-avatar tile class="mt-n7"   >
      <v-btn icon class="pink darken-1"><svg-icon type="mdi" :path="path11"></svg-icon></v-btn>
    </v-list-item-avatar>
    <v-list-item-content>
      <div class="overline text-left">จำนวนผู้ดูแลระบบ</div>
      <v-list-item-title class="headline mb-1 text-right" >{{ n11 }}</v-list-item-title>
      <div><v-divider></v-divider></div>
    </v-list-item-content> 
  </v-list-item>
  <v-card-actions>
  </v-card-actions>
</v-card>
    </v-flex>
    </v-layout>
  </v-container>
</div>

</template>

<script>
  import SvgIcon from '@jamescoyle/vue-icon';
  import { mdiCheckboxMultipleMarkedCircleOutline,mdiAlertCircle,mdiPlaylistCheck,mdiListBox 
    ,mdiShape,mdiAccountGroup,mdiAccountAlert,mdiAccountTie,mdiTabSearch
  ,mdiFileFind   } from '@mdi/js';
  
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
      cate:[],
      Eq:[],
      category:'',
      equip:'',
      wait: false,
      bor: false,
      re: false,
      path: mdiCheckboxMultipleMarkedCircleOutline ,
      path2: mdiAlertCircle,
      path3: mdiShape,
      path4: mdiPlaylistCheck ,
      path5: mdiListBox ,
      path6: mdiTabSearch, 
      path7: mdiFileFind ,
      path9: mdiAccountGroup ,
      path10: mdiAccountAlert,
      path11: mdiAccountTie,
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
    n4: 0,
    n5: 0,
    n6: 0,
    n7: 0,
    n8: 0,
    n9: 0,
    n10: 0,
    n11: 0,
      };
  },
  computed:{

  },
  created() {
  this.user = this.$auth.user
  this.getdataCate()
  this.getdataEqu()
  this.getNumeq()
  this.getNumBorring()
  this.getCate()
  this.getAllBorring()
  this.getWait() 

  this.getnumUser()
  this.getnotUser()
  this.getAdmin()
  },
  methods: {
    async getdataCate() {
    try {
      const res = await this.$axios.get(`/cate`);
      this.cate = res.data;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },

  async getdataEqu() {
    try {
      const res = await this.$axios.get(`/invs`);
      this.Eq = res.data;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },

      // number
  async getNumeq() {
    try {
      const res = await this.$axios.get(`/admin/allqty`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
      res.data.forEach(val => {
        this.n1 = this.n1 + val.i_qty
      });
      this.load = false;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getNumBorring() {
    try {
      const res = await this.$axios.get(`/admin/borrowqty`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
      res.data.forEach(val => {
        this.n2 = this.n2 + val.b_qty
      });
      this.load = false;
    }  catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getCate() {
    try {
      const res = await this.$axios.get(`/admin/Cate`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
        this.n3 = res.data.length;
      this.load = false;
    }  catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getnumUser() {
    try {
      const res = await this.$axios.get(`/admin/alluser`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
        this.n9 = res.data.length;
      this.load = false;
    }  catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getnotUser() {
    try {
      const res = await this.$axios.get(`/admin/notuser`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
        this.n10 = res.data.length;
      this.load = false;
    }  catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getAdmin() {
    try {
      const res = await this.$axios.get(`/admin/admin`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
        this.n11 = res.data.length;
      this.load = false;
    }  catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getAllBorring() {
    try {
      const res = await this.$axios.get(`/admin/BorAndRe`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
        this.n4 = res.data.length;
      this.load = false;
    }  catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getWait() {
    try {
      const res = await this.$axios.get(`/admin/Wait`);
      // eslint-disable-next-line no-console
      // console.log(this.bor);
        this.n5 = res.data.length;
      this.load = false;
    }  catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getnumcate(category){
    try {
      const res = await this.$axios.get(`/inv/cate/${category}`);
      this.n6 = res.data.length;
      this.load = false;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },

  async getnumeq(equip){
    try {
      const res = await this.$axios.get(`/inv/${equip}`);
      res.data.forEach(val => {
        this.n7 = val.i_qty;
      });
      
      this.load = false;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  }
  
},
}

</script>
