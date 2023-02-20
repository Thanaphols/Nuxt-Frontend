<!-- eslint-disable vue/valid-v-slot -->
<template>
    <v-layout row class="justify-center mt-1">
      <v-flex cols="12" sm="12"  md="12"  >
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
            <!-- <router-link :to="`/dashboard/addcategory`" > -->
                <v-btn  @click="dialog= !dialog"  class=""   depressed  color="primary" >
                 เพิ่มหมวดหมู่ 
                </v-btn>
            <!-- </router-link> -->
            </v-col>
          </v-row>
      </template>
      
      <template #item.actions="{ item }">
        <v-btn icon  outlined  class="mr-1 teal primary"  @click="updialog= !updialog; getUpCate(item.c_id)" >
            
            <v-icon small   >
        mdi-pencil
      </v-icon>
        </v-btn>
        <v-btn icon  outlined  class="mr-1 teal error"  @click="dedialog = !dedialog; opendeCate(item.c_id)">
            <v-icon >
        mdi-delete
      </v-icon>
        </v-btn>
    </template>

   

      
    </v-data-table>
    </v-flex>

    <v-dialog v-model="dialog" width="500px"> 
              <v-card class="pa-3" >
            <v-alert v-show="showAlert"  dense outlined  type="success">
          <v-row align="center">
            เพิ่มหมวดหมู่สำเร็จ
            <v-spacer/>
            <v-btn text plain @click="showAlert = !showAlert">X</v-btn>
          </v-row>
            
        </v-alert>
                
        <!-- Error Success -->
        <v-alert v-show="errorAlert"  dense outlined  type="error">
          <v-row align="center">
            ไม่สามารถเพิ่มหมวดหมู่ได้
            <v-spacer/>
            <v-btn text plain @click="errorAlert = !errorAlert">X</v-btn>
          </v-row>
            
        </v-alert>
                <v-card-title class="text-center"  >
                  
              <h2> เพิ่มหมวดหมู่ </h2>  <v-spacer/> <v-btn text plain @click="dialog = !dialog">X</v-btn>

            </v-card-title>
                <v-card-text>
                  <form  @submit.prevent="addCate()" >
        
          <v-row align="center">
            <v-col sm="4"></v-col>
            <v-col sm="6">
          </v-col>
          </v-row>
          
          <v-card-text >
            <v-row >
         <v-col sm="12" >
            <v-text-field    v-model="cate.c_name" min="0"  type="text" label="Category Name" outlined  ></v-text-field>
         </v-col>

         </v-row>
         </v-card-text>
  
         <v-row >
          <v-col sm="4">
          </v-col>
          <v-col sm="4">
            <v-card-actions  >
              <v-btn  type="submit">submit</v-btn>
              <v-btn @click="resetForm()">
                  clear
              </v-btn>
        </v-card-actions>  
          </v-col>
         </v-row>
         
         
        </form>
            </v-card-text>
          </v-card>
          
        </v-dialog>

        <v-dialog v-model="updialog" width="500px"> 
              <v-card class="pa-3" >
            <v-alert v-show="upshowAlert"  dense outlined  type="success">
          <v-row align="center">
            เพิ่มหมวดหมู่สำเร็จ
            <v-spacer/>
            <v-btn text plain @click="upshowAlert = !upshowAlert">X</v-btn>
          </v-row>
            
        </v-alert>
                
        <!-- Error Success -->
        <v-alert v-show="uperrorAlert"  dense outlined  type="error">
          <v-row align="center">
            ไม่สามารถเพิ่มหมวดหมู่ได้
            <v-spacer/>
            <v-btn text plain @click="uperrorAlert = !uperrorAlert">X</v-btn>
          </v-row>
            
        </v-alert>
                <v-card-title class="text-center"  >
                  
              <h2> แก้ไขหมวดหมู่ </h2>  <v-spacer/> <v-btn text plain @click="updialog = !updialog">X</v-btn>

            </v-card-title>
                <v-card-text>
                  <form  @submit.prevent="upCate(upcate_id)" >
       
          <v-flex row align="center">
            <v-col sm="4"></v-col>
            <v-col sm="6">
           
          </v-col>
          </v-flex>
          
          <v-card-text >
            <v-row >
         <v-col sm="12" >
            <v-text-field    v-model="upcate.c_name" min="0"  type="text" label="Category Name" outlined  ></v-text-field>
         </v-col>

         </v-row>
         </v-card-text>
  
         <v-row >
          <v-col sm="4">
          </v-col>
          <v-col sm="4">
            <v-card-actions  >
              <v-btn  type="submit">submit</v-btn>
        </v-card-actions>  
          </v-col>
         </v-row>
         
         
        </form>
            </v-card-text>
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
            ไม่สามารถลบได้เพราะมีอุปกรณ์ที่ใช้หมวดหมู่นี้
            <v-spacer/>
            <v-btn text plain @click="deerrorAlert = false">X</v-btn>
          </v-row>
            
        </v-alert>
          ยืนยันที่จะลบหมวดหมู่นี้หรือไม่
        </v-card-title>

        <v-card-text>
         
        </v-card-text>

        <v-card-actions>

          <v-btn
            color="green darken-1"
            text
            @click="confirmdeCate(de_id)"
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
  import { mdiKeyboardReturn } from '@mdi/js';
   export default {
    name: "CateGory",
    components: {
	},
    
    middleware: 'auth',
    data () {
      return {
        dialog: false,
        updialog: false,
        dedialog: false,
        path: mdiKeyboardReturn,
        u_id: '',
        data:[],
        up:[],
        delete:[],
        user:[],
        re:[],
        search: '',
        numall:  '',
        cate:{
        c_name: '',
      },
      
      showAlert: false,
      errorAlert: false,
      errorMessage:'',
      loginMessage:'',
      upshowAlert: false,
      uperrorAlert: false,
      upcate:{
        c_id:'',
        c_name: '',
      },
      deerrorAlert: false,
      de_id: '',
      }
    },
    
    computed: {
        
      headers () {
        return [
          
          { text: 'ไอดีหมวดหมู่', value: 'c_id', filter: value => {
              if (!this.b_id) return true
              return value < parseInt(this.b_id)
            }, },
          { text: 'ชื่อหมวดหมู่', value: 'c_name' },
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
           // eslint-disable-next-line no-console
          // console.log(this.u_id)
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
           this.dedialog = false
            this.getmecate()
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);
            this.deerrorAlert = true
            }
        },

        

    async reBorrow(id,qty,id2) {
        
        try {
        const res = await this.$axios.patch(`/borrow/return/${id}/${qty}/${id2}`);
        this.re = res.data;
        // eslint-disable-next-line no-console
        // console.log(this.re);
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
            // console.log(this.data);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },

        async addCate() {
          await this.$axios.post(`/cate/addCate/`,{
        c_name: this.cate.c_name,
          
        })
        .then((response) => {
          this.errorAlert = false
          this.showAlert = true
          // eslint-disable-next-line no-console
          console.log(response);
           this.loginMessage =  response.data;
           this.getmecate()
        })
        .catch((error)=> {
          // eslint-disable-next-line no-console
          console.error(error);
          this.showAlert = false
          this.errorAlert = true
          this.errorMessage =  error.response.data;
         
        })
       
      },

      async getUpCate(id) {
      try {
        const res = await this.$axios.get(`/cate/${id}`);
        res.data.forEach(val => {
          this.upcate.c_id = val.c_id;
          this.upcate.c_name = val.c_name;
        });

        // eslint-disable-next-line no-unused-expressions
        
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
  
      }
    },

    async upCate() {
          await this.$axios.patch(`/cate/upCate/${this.upcate.c_id}`,{
        c_name: this.upcate.c_name,
          
        })
        .then((response) => {
          this.uperrorAlert = false
          this.upshowAlert = true
          
           this.getmecate()
        })
        .catch((error)=> {
          // eslint-disable-next-line no-console
          console.error(error);
          this.upshowAlert = false
          this.uperrorAlert = true
          this.errorMessage =  error.response.data;
         
        })
      },

      opendeCate(id){
          this.dedialog = true
          this.de_id = id
        //  console.log(this.de_id)
         
        //  this.deInv(id)
        },

        confirmdeCate(id){
           
         this.deCate(id)
        },

      resetForm(){
                this.cate.c_name="";
      },

    },
   
  }
  </script>