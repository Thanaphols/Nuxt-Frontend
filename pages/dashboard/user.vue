<!-- eslint-disable vue/valid-v-slot -->
<template>
    
    <v-layout row class="justify-center mt-1">
      <v-col cols="12" sm="12"  md="12"  >
    <v-data-table :headers="headers" :items="data"  class="elevation-1 center" :search="search" cols="12" sm="12"  md="12" :custom-filter="filterOnlyCapsText" >
   
      <template #top>

        <v-row  align="center">
            <v-flex  col4 md6 sm12 xs12>
                <v-text-field  v-model="search"  label="Search (UPPER CASE ONLY)" class="mx-5 " ></v-text-field>
            </v-flex>
            <v-flex col4 md6 sm12 xs12>
                <v-btn  plain text   >
                จำนวนผู้ใช้งานทั้งหมด 
                {{numall}}
                </v-btn>
                
                <v-btn @click="dialog = !dialog"  outlined dark class="primary" >
                เพิ่มผู้ใช้งาน
                </v-btn>
            </v-flex>
            
        </v-row>
      </template>

      <template #item.actions="{ item }">
        <v-btn icon  outlined  class="mr-1 primary" @click="getUser(item.u_id);updialog=!updialog">
            <v-icon small   >
        mdi-pencil
      </v-icon>
        </v-btn>
        <v-btn   icon  outlined  class="mr-1 error" @click="opendeUser(item.u_id)">
            <v-icon >
        mdi-delete
      </v-icon>
        </v-btn>
      
    </template>

      
    </v-data-table>
    </v-col>

    <v-dialog v-model="dialog" width="500px"> 
              <v-card class="pa-3" >
            <v-alert v-show="showAlert"  dense outlined  type="success">
          <v-row align="center">
            Add User Success
            <v-spacer/>
            <v-btn text plain @click="showAlert = !showAlert">X</v-btn>
          </v-row>
            
        </v-alert>
                
        <!-- Error Success -->
        <v-alert v-show="errorAlert"  dense outlined  type="error">
          <v-row align="center">
            {{errorMessage}}
            <v-spacer/>
            <v-btn text plain @click="errorAlert = !errorAlert">X</v-btn>
          </v-row>
            
        </v-alert>
                <v-card-title class="text-center"  >
                  
              <h2> เพิ่มผู้ใช้งาน </h2>  <v-spacer/> <v-btn text plain @click="dialog = !dialog">X</v-btn>

            </v-card-title>
                <v-card-text>
                  <form  @submit.prevent="addUser()" >

    <v-text-field  v-model="add.email"  type="email" :counter="50"  label="อีเมล"   outlined
      > </v-text-field>
      <v-text-field  v-model="add.username"  :counter="50" label="Username"  outlined
      > </v-text-field>
      
      <v-row>
        <v-col cols="12" sm="6" >
          <v-text-field  v-model="add.firstname"  :counter="50" label="ชื่อจริง"  outlined > 
      </v-text-field>
        </v-col>
        <v-col cols="12" sm="6" >
        <v-text-field  v-model="add.lastname"  :counter="50" label="นามสกุล"  outlined> 
      </v-text-field>
        </v-col>
      </v-row>

      <v-text-field  v-model="add.password" type="password" max=8  :counter="8"   label="รหัสผ่าน"  outlined
      > </v-text-field>
      <v-card-actions  >
        
      <v-btn class="primary"  type="submit"  >
          ยืนยัน
      </v-btn>
      <v-btn class="warning" @click="resetForm()">
          ล้างข้อมูล
      </v-btn>
    </v-card-actions>
    </form>
            </v-card-text>
          </v-card>
          
        </v-dialog>
        
        <v-dialog v-model="updialog" width="500px"> 
              <v-card class="pa-3" >
            <v-alert v-show="upshowAlert"  dense outlined  type="success">
          <v-row align="center">
            Update User Success
            <v-spacer/>
            <v-btn text plain @click="upshowAlert = !upshowAlert">X</v-btn>
          </v-row>
            
        </v-alert>
                
        <!-- Error Success -->
        <v-alert v-show="uperrorAlert"  dense outlined  type="error">
          <v-row align="center">
            {{uperrorMessage}}
            <v-spacer/>
            <v-btn text plain @click="uperrorAlert = !uperrorAlert">X</v-btn>
          </v-row>
            
        </v-alert>
                <v-card-title class="text-center"  >
                  
              <h2> แก้ไขข้อมูลผู้ใช้งาน </h2>  <v-spacer/> <v-btn text plain @click="updialog = !updialog">X</v-btn>

            </v-card-title>
                <v-card-text>
                  <form  @submit.prevent="upUser(up.u_id)" >
 
                    <v-text-field  v-model="up.email"  type="email" :counter="50" label="Email" 
                      > </v-text-field>
                      <v-text-field  v-model="up.username"  :counter="50" label="Username" 
                      > </v-text-field>

                      <v-text-field  v-model="up.firstname"  :counter="50" label="firstname" > 
                      </v-text-field>
                      <v-text-field  v-model="up.lastname"  :counter="50" label="Lastname" > 
                      </v-text-field>
                      <v-select  v-model="up.u_stat"  :items="items"  label="user Status"  ></v-select>
                      <v-card-actions  >
                        
                    <v-btn  type="submit"  >
                      ยืนยัน
                    </v-btn>
                    </v-card-actions>
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
            ไม่สามารถลบได้เพราะผู้ใช้นี้มีรายการยืม-คืน อยู่
            <v-spacer/>
            <v-btn text plain @click="deerrorAlert = false">X</v-btn>
          </v-row>
            
        </v-alert>
          ยืนยันที่จะลบผู้ใช้งานนี้หรือไม่
        </v-card-title>

        <v-card-text>
         
        </v-card-text>

        <v-card-actions>

          <v-btn
            color="green darken-1"
            text
            @click="confirmdeUser(de_id)"
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
        dialog: false,
        updialog: false,
        dedialog: false,
        u_id: '',
        data:[],
        delete:[],
        search: '',
        i_id: '',
        de_id:'',
        numall:'',
        showAlert: false,
        errorAlert: false,
        upshowAlert: false,
        uperrorAlert: false,
        errorMessage:'',
        uperrorMessage:'',
        upMessage:'',
        deerrorAlert: false,

        user: {
                    email:"",
                    username:"",
                    firstname:"",
                    lastname:"",
                    password:"",
                  },
        add: {
                    email:"",
                    username:"",
                    firstname:"",
                    lastname:"",
                    password:"",
                  },
                  up: {
                    email:"",
                    username:"",
                    firstname:"",
                    lastname:"",
                    password:"",
                  },
        items: ['NotApproved', 'Approved', 'Admin'],          
       
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
          this.user = this.$auth.user
           this. u_id = this.user.u_id
          this.getAllUser()
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
            const res = await this.$axios.delete(`/admin/deUser/${id}`);
            this.delete = res.data;
            // eslint-disable-next-line no-console
            this.dedialog = false
            this.getAllUser()
            
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);
            this.deerrorAlert = true
            }
        },

      async getAllUser() {
            try {
            const res = await this.$axios.get(`/users/`);
            this.data = res.data;
            this.numall = res.data.length;
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
        },
        async addUser() {
        await this.$axios.post(`/users/addUser/`,{
        email: this.add.email,
        username: this.add.username,
        firstname: this.add.firstname,
        lastname: this.add.lastname,
        password: this.add.password,
      })
      .then((response) => {
        this.errorAlert = false
        this.showAlert = true
        // eslint-disable-next-line no-console
        console.log(response);
        this.regisMessage =  response.data.message;
        this.getAllUser()
      })
      .catch((error)=> {
        // eslint-disable-next-line no-console
        console.error(error);
        this.showAlert = false
        this.errorAlert = true
        this.errorMessage =  error.response.data.message;
       
      })
     
    },

    async getUser(id){
                try {
            const res = await this.$axios.get(`/user/${id}`);
            
            res.data.forEach(val => {
                
                this.up.u_id = val.u_id;
                this.up.email = val.email;
                this.up.username = val.username;
                this.up.firstname = val.firstname;
                this.up.lastname = val.lastname;
                this.up.u_stat= val.u_stat;
            });
            
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
          },

          async upUser(id) {
          await this.$axios.patch(`/admin/adupUser/${id}`,{
          email: this.up.email,
          username: this.up.username,
          firstname: this.up.firstname,
          lastname: this.up.lastname,
          u_stat: this.up.u_stat,
          
        })
        .then((response) => {
          this.uperrorAlert = false
          this.upshowAlert = true
          // eslint-disable-next-line no-console
          this.upMessage =  response.data;
          this.getAllUser()
        })
        .catch((error)=> {
          // eslint-disable-next-line no-console
          console.error(error);
          this.upshowAlert = false
          this.uperrorAlert = true
          this.uperrorMessage =  error.response;
         
        })
       
      },

        opendeUser(id){
          this.dedialog = true
          this.de_id = id
        //  console.log(this.de_id)
         
        //  this.deInv(id)
        },

        confirmdeUser(id){
          
         this.deInv(id)
        },

     resetForm(){
              this.showAlert = false
              this.errorAlert = false
              this.add.email="";
              this.add.username="";
              this.add.firstname="";
              this.add.lastname="";
              this.add.password="";
    }
    },
  }
  </script>