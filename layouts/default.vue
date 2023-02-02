<template>
  
  <v-app >
    <v-row class="mt-0">
    <v-app-bar >
      <v-app-bar-nav-icon class="ml-2" @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-app-bar-title>
        <router-link to="/">Equipments Manage System </router-link>
      </v-app-bar-title>
      <v-spacer/>
      <button  v-if="user"  class="mr-2"  @click="logout()"><span class="material-icons"><svg-icon type="mdi" :path="path"></svg-icon></span> </button>

      <router-link  v-if="!user" :to="`/auth/`"><span class="material-icons"><svg-icon type="mdi" :path="path2"></svg-icon></span> </router-link>
      
    </v-app-bar>
  </v-row>

    <v-navigation-drawer v-model="drawer" absolute  bottom temporary >
      <v-list-item title="Regis"  href="/auth/regis/" >Regis</v-list-item>
      <v-list-item v-if="!user"    title="Login"   href="/auth/"><svg-icon type="mdi" :path="path2"></svg-icon></v-list-item>
      <v-list-item title="Profile"  href="/auth/profile/" >Profile</v-list-item>
      <v-list-item title="Borrow"  href="/borrow/" >borrow</v-list-item>
      <v-list-item title="MeBorrow"  href="/borrow/meborrow" >BorrowMe</v-list-item>
      <v-list-item title="ReturnMe"  href="/borrow/meReturn" >ReturnMe</v-list-item>
      <!-- <v-list-item title="equipment"  href="/inv/equipment" >Equipment</v-list-item> -->
      <v-list-item title="AdminDashboard"  href="/dashboard/" >AdminDashboard</v-list-item>
      <v-list-item title="AdminUser"  href="/dashboard/user" >AdminUser</v-list-item>
      <v-list-item title="AdminEquipment"  href="/dashboard/equipment" >AdminEquipment</v-list-item>
      <v-list-item title="AdminBorrow"  href="/dashboard/borrow" >AdminBorrow</v-list-item>
      <v-list-item title="AdminReturn"  href="/dashboard/return" >AdminReturn</v-list-item>
      <v-list-item title="AdminCategory"  href="/dashboard/category" >AdminCategory</v-list-item>
      <!-- <v-list-item title="Regis"  href="/regis/" >logout</v-list-item> -->
    </v-navigation-drawer>
    
    <v-main>
      <nuxt/>
    </v-main>
  </v-app>
</template>

<script>
import SvgIcon from '@jamescoyle/vue-icon';
import { mdiLogoutVariant,mdiAccount } from '@mdi/js';
export default {
  
  name: 'App',
  components: {
    SvgIcon
  },
  data: () => ({
    user: [],
    path: mdiLogoutVariant,
    path2: mdiAccount,
      drawer: false,
      bar: false,
  }),
  created(){
      this.user = this.$auth.user
  },
  methods:  {

        async logout () { 
          await this.$auth.logout()
          this.$router.go('/')
        },

         login () { 
          // this.$router.go()
          this.$router.push('/auth')
         
        }
    }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
a {
  text-decoration: none;
  color: black;
}
</style>
