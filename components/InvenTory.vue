<!-- eslint-disable vue/prop-name-casing -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/prop-name-casing -->
<template>
    <v-card class="ma-3 rounded-lg " warp light elevation="11" outlined >
      <div class="black">
        <v-img
      v-if=" i_img === null" 
      max-height="250"
      height="250"
      cover
      :src="require(`~/assets/images/noimg.png`)"
    ></v-img>
    <v-img
      v-else
      :aspect-ratio="1"
      max-height="250"
      height="100%"
      cover
      :src="require(`~/assets/images/${i_img}`)"
    ></v-img>
      </div>
      <v-card-title class="el"><div class="mul">
          ชื่ออุปกรณ์ : {{ i_name }}
          </div></v-card-title>
      <v-card-text>
        <v-row align="center" class="mx-0"  >
        <div class="my-4 text-subtitle-1">
          <div v-for=" ca in cate " :key="ca.c_id" >
            <div v-if=" i_category == ca.c_id">
              <p>หมวดหมู่ : {{ ca.c_name }}</p>
            </div>
          </div>
          <p> จำนวน :  {{i_qty}}</p>
        </div>
        </v-row>
      </v-card-text>
      <v-divider class="mx-4"></v-divider>
      <v-card-actions>
        <router-link :to="`/inv/invdetail/${i_id}`" >
            <v-btn color="teal  lighten-2" text  >
              รายละเอียด
            </v-btn>
        </router-link>
      </v-card-actions>
    </v-card>
 
    

  </template>
<script>
export default {
    name: 'InvenTory',
   
    
    props:{
      // eslint-disable-next-line vue/require-default-prop, vue/prop-name-casing
      i_id:{
        type: Number,
      },
      // eslint-disable-next-line vue/require-default-prop, vue/prop-name-casing
      i_name:{
            type: String,
        },
        // eslint-disable-next-line vue/require-default-prop, vue/prop-name-casing
      i_category:{
            type: Number,
        },
        // eslint-disable-next-line vue/require-default-prop, vue/prop-name-casing
      i_stat:{
            type: String,
        },
        // eslint-disable-next-line vue/require-default-prop, vue/prop-name-casing
      i_qty:{
            type: Number,
        },
        // eslint-disable-next-line vue/require-default-prop, vue/prop-name-casing
      i_img:{
            type: String,
        },
    },
    data () {
      return {
        cate:{
        c_id: '',
        c_name: '',
       },
      }
    },
    mounted(){
        this.getcate()
    },
    methods:{
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
    }
    
    
  }
</script>
<style>
.el {
  white-space: nowrap;
  width: 250px;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>