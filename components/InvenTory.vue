<!-- eslint-disable vue/prop-name-casing -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/require-default-prop -->
<!-- eslint-disable vue/prop-name-casing -->
<template>
    <v-card
      
      class="mx-auto "
      width="355"
    >
    <v-img
      v-if=" i_img === null" 
      :aspect-ratio="1"
      height="250"
      cover
      :src="require(`~/assets/images/noimg.png`)"
    ></v-img>
    <v-img
      v-else
      :aspect-ratio="1"
      height="250"
      cover
      :src="require(`~/assets/images/${i_img}`)"
    ></v-img>
   
    
  
      <v-card-title class="el"><div class="mul">
          EqName : {{ i_name }}
          </div></v-card-title>
  
      <v-card-text>
        <v-row
          align="center"
          class="mx-0"
        >
  
        <div class="my-4 text-subtitle-1">
          <p>Catagory : {{ i_category }}</p>
          <p> Emaining :  {{i_qty}}</p>
          <p> Status :  {{i_stat}}</p>
        </div>
        </v-row>
  
      </v-card-text>
  
      <v-divider class="mx-4"></v-divider>
  
  
      <v-card-actions>
        <router-link :to="`/inv/invdetail/${i_id}`" >
            <v-btn color="teal  lighten-2" text  >
              Details
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
    mounted(){
      // console.log(this.i_img)
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