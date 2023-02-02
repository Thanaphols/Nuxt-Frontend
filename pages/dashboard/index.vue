<template>
    <v-container  >
      <div v-show="load" class="display-center">
        <v-progress-circular indeterminate></v-progress-circular>
      </div>
  
      <v-row class="mt-3  display-center">
        <v-col sm="4">
          <v-card >
        <v-card-title>
          จำนวนอุปกรณ์ทั้งหมด {{ n }}
          <v-spacer/>
        </v-card-title>
        
      </v-card>
        </v-col>
        <v-col sm="4">
          <v-card >
        <v-card-title>
          รายการอุปกรณ์ทั้งหมดที่กำลังถูกยืม {{ n2 }}
          <v-spacer/>
        </v-card-title>
        
      </v-card>
        </v-col>
        <v-col sm="4">
          <v-card >
        <v-card-title>
          รายการอุปกรณ์ทั้งหมดที่ถูกคืนแล้ว {{ n3 }}
          <v-spacer/>
        </v-card-title>
        
      </v-card>
        </v-col>
      </v-row>
      
      <v-row class="mb-6 display-center" dense>
        
        
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
      num: 6,
      data: [],
      bor:[],
      re:[],
      load: true,
        };
    },
    created() {
    this.getProduct();
    this.getBorrow();
    this.getReturn();
    },
    methods: {
    async getProduct() {
      try {
        const res = await this.$axios.get(`/inv`);
        this.data = res.data;
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
        this.bor = res.data;
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
        this.re = res.data;
        // eslint-disable-next-line no-console
        // console.log(this.re);
        this.n3 = res.data.length;
        this.load = false;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
  },
  }
  
  </script>