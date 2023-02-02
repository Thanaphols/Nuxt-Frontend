<!-- eslint-disable vue/prop-name-casing -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/prop-name-casing -->
<template>
    <v-card
      class="mx-auto my-12"
      width="350"
    >
    <v-img
      height="200" 
      :src=" i_img"
    ></v-img>
  
      <v-card-title class="el"><div class="mul">
          ชื่ออุปกรณ์ : {{ i_name }}
          </div></v-card-title>
  
      <v-card-text>
        <v-row
          align="center"
          class="mx-0"
        >

        <div class="my-4 text-subtitle-1">
          <p>ชนิดอุปกรณ์ : {{ i_category }}</p>
          <p> สถานะอุปกรณ์ :  {{i_stat}}</p>
        </div>
        </v-row>
  
      </v-card-text>
  
      <v-divider class="mx-4"></v-divider>
  
  
      <v-card-actions>
        <router-link :to="`/inv/${i_id}`" >
            <v-btn color="deep-purple lighten-2" text  >
            รายละเอียด
            </v-btn>
        </router-link>
      </v-card-actions>
    </v-card>
  </template>
<script>
export default {

    name: 'BorRow',
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
            type: String,
        },
        // eslint-disable-next-line vue/require-default-prop, vue/prop-name-casing
      i_stat:{
            type: String,
        },
        // eslint-disable-next-line vue/require-default-prop, vue/prop-name-casing
      i_img:{
            type: String,
        },
      
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