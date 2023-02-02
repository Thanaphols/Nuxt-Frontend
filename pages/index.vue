<template>
  <v-container>
    <div v-show="load" class="display-center">
      <v-progress-circular indeterminate></v-progress-circular>
    </div>

    <v-row class="mt-3">
      <v-col>
        <v-card >
      <v-card-title>
        จำนวนอุปกรณ์ทั้งหมด {{ n }}
        <v-spacer/>
      </v-card-title>
      
    </v-card>
      </v-col>
    </v-row>
    
    <v-row class="mb-6 display-center" dense>
      
      <div v-for="item in data" :key="item.id">
      <v-expand-transition>
        <v-col cols="auto">
          <InvenTory
            :i_name="item.i_name"
            :i_category="item.i_category"
            :i_stat="item.i_stat"
            :i_qty="item.i_qty"
            :i_id="item.i_id"
            :i_img="item.i_img"
          />
        </v-col>
      </v-expand-transition>
      </div>
    </v-row>
  </v-container>
</template>

<script>
import InvenTory from "../components/InvenTory.vue";
export default {
  

  name: "HomeView",
  components:{
    InvenTory,
  },
  data() {
  return {
    n: '',
    num: 6,
    data: [],
    load: true,
      };
  },
  created() {
  this.getProduct();
  },
  methods: {
  async getProduct() {
    try {
      const res = await this.$axios.get(`/inv`);
      this.data = res.data;
      // eslint-disable-next-line no-console
      console.log(this.data);
      this.n = res.data.length;
      this.load = false;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
    }
  },
},
}

</script>