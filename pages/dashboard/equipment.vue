<!-- eslint-disable vue/valid-v-slot -->
<template>
    
    <v-layout row class="justify-center mt-1">
      <v-flex cols="12" sm="12"  md="12" >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" :custom-filter="filterOnlyCapsText" >
      
      <template #top>
        <v-layout row  align="center">
          <v-flex  col4 md6 sm12 xs12>
                <v-text-field  v-model="search"  label="Search (UPPER CASE ONLY)" class="mx-5 " ></v-text-field>
            </v-flex>
            <v-flex col4 md6 sm12 xs12>
                <v-btn  plain text   >
                จำนวนอุปกรณ์ทั้งหมด
                {{numall}}
                </v-btn>
                
                <router-link :to="`/dashboard/addequipment`" >
                <v-btn depressed  color="primary" >
                 เพิ่มอุปกรณ์
                </v-btn>
            </router-link>
            </v-flex>
        </v-layout>
      </template>

      <template #item.category="{ item }">
        
        <div v-for=" ca in cate " :key="ca.c_id" >
          
       {{  item.i_category == ca.c_id ? ca.c_name : ''}}
    </div>
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
     
      
      <template #item.actions="{ item }">
        <router-link :to="`/dashboard/${ item.i_id }`" >
        <v-btn icon  outlined  class="mr-1 primary">
            <v-icon>
        mdi-pencil
      </v-icon>
        </v-btn>
    </router-link>
        <v-btn icon  outlined  class="mr-1 primary error"  @click="dedialog = !dedialog;openDe(item.i_id)">
            <v-icon >
        mdi-delete
      </v-icon>
        </v-btn>
      
    </template>

      
    </v-data-table>
    </v-flex>

    <v-dialog
      v-model="dedialog"
      max-width="380"
    >
      <v-card align="center">
       
                
        <!-- Error Success -->
        <v-alert v-show="deerrorAlert"  dense outlined  type="error">
          <v-row align="center">
            ไม่สามารถลบรายการคืนนี้ได้
            <v-spacer/>
            <v-btn text plain @click="deerrorAlert = false">X</v-btn>
          </v-row>
            
        </v-alert>
        <v-card-title   class="text-h5">
         
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
   export default {
    middleware: 'auth',
    data () {
      return {
        u_id: '',
        data:[],
        search: '',
        i_id: '',
       numall:'',
       cate:{
        c_id: '',
        c_name: '',
       },
       de:[],

       dedialog: false,
        deerrorAlert: false,
        d_id: '',
      }
    },
    
    computed: {
        
      headers () {
        return [
          
          { text: 'ไอดีอุปกรณ์', value: 'i_id', filter: value => {
              if (!this.i_id) return true
              return value < parseInt(this.i_id)
            }, },
          { text: 'ชื่ออุปกรณ์', value: 'i_name' },
          { text: 'จำนวน', value: 'i_qty'  },
          { text: 'หมวดหมู่', value: 'category'  },
          { text: 'รูปภาพ', value: 'img' },
          { text: 'จัดการ', value: 'actions', sortable: false },
          
        ]
      },
    },
    mounted() {
          // eslint-disable-next-line no-console
          // console.log(this.desserts)
          this.user = this.$auth.user
           this. u_id = this.user.u_id
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
            const res = await this.$axios.delete(`/admin/deInv/${id}`);
            this.de = res.data;

            this.dedialog = false
            this.getEq()
            
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);
            this.dedialog = true
            this.deerrorAlert = true

            }
        },
    
        

      async getEq() {
            try {
            const res = await this.$axios.get(`/inv/`);
            this.data = res.data;
            this.numall = this.data.length;
            // eslint-disable-next-line no-console
            // console.log(this.data);
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
            // console.log(this.cate);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

        openDe(id){
          this.dedialog = true
          this.d_id = id
          // console.log(id,qty,id2)
        },

        confirmDe(id){
          // console.log()
        this.deInv(id)
        },

    },
  }
  </script>