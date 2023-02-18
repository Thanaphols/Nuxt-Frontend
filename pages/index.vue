<template>
  <v-container>
    
    <div v-show="load" >
      <v-progress-circular indeterminate></v-progress-circular>
    </div>
    <v-layout row align-center >
      <v-flex class="" sm4 xs5 md4 lg3 >
           <v-btn text dark class="primary"> <h3>จำนวนอุปกรณ์ {{ n }}</h3></v-btn> 
      </v-flex>
      <v-flex class="" sm4 xs3 md4 lg5 >
        <v-autocomplete
        v-model="category" :items="cate" item-value="c_id"  item-text="c_name" density="compact"  width="100"    label="ค้นหาตามหมวดหมู่"
         @change="getincate(category)" >
        </v-autocomplete>
        
      </v-flex>
      <v-flex class=""  sm4 xs4 md4 lg4 >
        <v-btn class="primary ml-2" @click="getProduct()">แสดงทั้งหมด</v-btn>
        
      </v-flex>
       
    </v-layout>
    
    <v-layout v-show="all" row warp>
      <v-flex v-for="item in data" :key="item.id" sm6 xs12 md4 lg4 >
          <v-expand-transition>
        <InvenTory
          :i_name="item.i_name"
          :i_category="item.i_category"
          :i_stat="item.i_stat"
          :i_qty="item.i_qty"
          :i_id="item.i_id"
          :i_img="item.i_img"
        />
        </v-expand-transition>
    </v-flex>
    </v-layout>

    <v-layout v-show="incate" row warp>
      <v-flex v-for="item in data" :key="item.id" sm6 xs12 md4 lg4 >
          <v-expand-transition>
        <InvenTory
          :i_name="item.i_name"
          :i_category="item.i_category"
          :i_stat="item.i_stat"
          :i_qty="item.i_qty"
          :i_id="item.i_id"
          :i_img="item.i_img"
        />
        </v-expand-transition>
    </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
// import InvenTory from "../components/InvenTory.vue";
export default {
  

  name: "HomeView",
  components:{
    // InvenTory,
  },
  data() {
  return {
    select:'',
    n: '',
    num: 6,
    cate:[],
    data: [],
    load: true,
    category:'',
    all: true,
    incate:false
      };
  },
  created() {
  this.getProduct();
  this.getCate()
  },
  methods: {
  async getProduct() {
    try {
      this.incate =false
      this.all= true
      this.category = ''
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
  async getCate() {
    try {
      const res = await this.$axios.get(`/cate`);
      this.cate = res.data;
      
      // eslint-disable-next-line no-console
      // console.log(this.data);
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
  async getincate(category){
    try {
      this.incate =true
      this.all= false
      const res = await this.$axios.get(`/inv/cate/${category}`);
      this.data = res.data;
      // eslint-disable-next-line no-console
      // console.log(this.data);
      this.n = res.data.length;
      this.load = false;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  }
},
}

</script>