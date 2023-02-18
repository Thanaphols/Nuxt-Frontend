<template>
   <v-layout row warp >
    <v-flex lg3 md3 sm3></v-flex>
    <v-flex lg6 md6 sm6>
    <v-card v-for="(item, i) in data" :key="i" v-card class="ma-3 rounded-lg " warp light elevation="11" outlined width="100%"  >
      <div class="black">
      <v-img
      v-if=" item.i_img === null" 
      :aspect-ratio="1"
      class="bg-grey-lighten-2"
      circle
      :src="require(`~/assets/images/noimg.png`)"
    ></v-img>
    <v-img
      v-else
      :aspect-ratio="1"
      circle
      :src="require(`~/assets/images/${item.i_img}`)"
    ></v-img>
  </div>
        <v-card-title
          ><div class="mul">
          Equipment Name : {{ item.i_name }}
          </div>
        </v-card-title>
      <v-card-text>
        <v-row align="center" class="mx-0">
        <div class="my-4 text-subtitle-1">
          <div v-for=" ca in cate " :key="ca.c_id" >
            <div v-if=" item.i_category === ca.c_id">
              <p>Catagory : {{ ca.c_name }}</p>
            </div>
          </div>
          <p>Quetity : {{ item.i_qty }}</p>
        </div>
        </v-row>
        <div>
        </div>
      </v-card-text>
      <v-card-actions>
      <router-link  :to="`/`">
        <v-btn color="primary"  depressed  >
              ย้อนกลับ
            </v-btn>
    </router-link>
  </v-card-actions>
    </v-card>
  </v-flex>
  <v-flex lg3 md3 sm3></v-flex>
  </v-layout>
  
</template>

<script>
// eslint-disable-next-line no-unused-vars
export default {
  // middleware: 'auth',
  data() {
    return {
      data: [],
      cate:[],
    };
  },
  async created() {
    await this.getData();
    // console.log(this.data);
  },
  mounted() {
    this.getcate();
  },
  methods: {
    async getData() {
      try {
        const resp = await this.$axios.get(
          `/inv/${this.$route.params.i_id}`
        );
        this.data = resp.data;
        // eslint-disable-next-line no-console
        console.log('DATA : ',this.data);
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
    async getcate(){
        try {
            const res = await this.$axios.get(`/cate/`);
            this.cate = res.data;
            // eslint-disable-next-line no-console
           // console.log(this.cate);
            } catch (e) {
            // eslint-disable-next-line no-console
            console.error(e);

            }
      }
  },
};
</script>

<style>
.display-center {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.mul {
  overflow-wrap: break-word;
}
</style>