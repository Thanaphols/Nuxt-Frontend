<template>
    <v-container  >
      
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
    </v-container>
  </template>
  
  <script>
  export default {
    
  
    name: "HomeView",
    components:{
     
    },
    data() {
    return {
      n: '',
      n2: '',
      n3: '',
      n4: '',
      n5: '',
      n6: '',
      n7: '',
      n8: '',
      n9: '',

        };
    },
    created() {
    this.getEq();
    this.getBorrow();
    this.getReturn();
    this.getWait();
    this.getUser();
    this.getUserNot();
    this.getUserApp();
    this.getUserAdmin();
    this.getCate();
    },
    methods: {
    async getEq() {
      try {
        const res = await this.$axios.get(`/inv`);
        // eslint-disable-next-line no-console
        // console.log(this.data);
        this.n = res.data.length;
        this.load = false;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getBorrow() {
      try {
        const res = await this.$axios.get(`/borrow`);
        // eslint-disable-next-line no-console
        // console.log(this.bor);
        this.n2 = res.data.length;
        this.load = false;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getReturn() {
      try {
        const res = await this.$axios.get(`/return`);
        // eslint-disable-next-line no-console
        // console.log(this.re);
        this.n3 = res.data.length;
        this.load = false;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getWait() {
      try {
        const res = await this.$axios.get(`/wait`);
        // eslint-disable-next-line no-console
        // console.log(this.re);
        this.n4 = res.data.length;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getUser() {
      try {
        const res = await this.$axios.get(`/users`);
        // eslint-disable-next-line no-console
        // console.log(this.re);
        this.n5= res.data.length;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getUserNot() {
      try {
        const res = await this.$axios.get(`/users/not`);
        // eslint-disable-next-line no-console
        // console.log(this.re);
        this.n6= res.data.length;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getUserApp() {
      try {
        const res = await this.$axios.get(`/users/app`);
        this.n7= res.data.length;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getUserAdmin() {
      try {
        const res = await this.$axios.get(`/users/admin`);
        this.n8= res.data.length;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getCate() {
      try {
        const res = await this.$axios.get(`/cate`);
        this.n9= res.data.length;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
  },
  }
  
  </script>