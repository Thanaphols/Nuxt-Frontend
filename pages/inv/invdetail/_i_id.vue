<template>
  <v-container>
    <div v-show="load" class="display-center">
      <v-progress-circular indeterminate></v-progress-circular>
    </div>
    <v-card v-for="(item, i) in data" :key="i" class="mx-auto my-12" max-width="500">
      <v-img
      height="200" 
      :src=" item.i_img"
    ></v-img>
        <v-card-title
          ><div class="mul">
          ชื่ออุปกรณ์ : {{ item.i_name }}
          </div>
        </v-card-title>


      <v-card-text>
        <v-row align="center" class="mx-0">
          <p >
         
        </p>

        <div class="my-4 text-subtitle-1">
         <p> ชนิดอุปกรณ์ : {{ item.i_category }}</p>
         <p> สถานะอุปกรณ์ : {{ item.i_stat }} </p>
        </div>
          
        </v-row>


        <div>
        </div>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
// eslint-disable-next-line no-unused-vars
export default {
  // middleware: 'auth',
  data() {
    return {
      data: [],
      load: true,
    };
  },
  async created() {
    await this.getData();
    // console.log(this.data);
  },
  mounted() {},
  methods: {
    async getData() {
      try {
        const resp = await this.$axios.get(
          `/inv/${this.$route.params.i_id}`
        );
        this.data = resp.data;
        // eslint-disable-next-line no-console
        console.log('DATA : ',this.data);
        this.load = false;
      } catch (e) {
        // eslint-disable-next-line no-console
        console.error(e);
      }
    },
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