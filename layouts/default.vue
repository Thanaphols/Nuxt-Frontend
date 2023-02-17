<template>
  
  <v-app  >
    <v-app-bar app color="primary"  dark >
      <v-app-bar-nav-icon class="ml-2" @click="drawer = !drawer"></v-app-bar-nav-icon>
      
      <v-app-bar-title >
        <router-link  style="text-decoration: none; color: inherit;"  to="/">Equipments Manage System 
      </router-link>
        
      </v-app-bar-title>
    
      <v-spacer/>
      <div >
        <v-btn  v-show="user" class="mr-2"  @click="logout()"><span class="material-icons"><svg-icon type="mdi" :path="path"></svg-icon></span> </v-btn>
        <!-- <router-link  v-show="!user" :to="`/auth/`"><span class="material-icons"><svg-icon type="mdi" :path="path2"></svg-icon></span> </router-link> -->
      </div>
      <v-switch
        v-model="$vuetify.theme.dark"
        inset
        persistent-hint
        class="mt-5"
      ></v-switch>
    </v-app-bar>
 
    <v-navigation-drawer v-model="drawer"   app  bottom temporary  >
      <router-link  v-show="!user" :to="`/auth/`">
      <v-list-item  v-show="!user"    title="Login" >
          <svg-icon type="mdi" class="mb-0" :path="path2"> </svg-icon> <span class="ml-2">Login</span>
      </v-list-item>
    </router-link>
      <router-link  v-show="!user" :to="`/auth/Regis/`">
      <v-list-item title="Regis"   >
        <span >Regis</span> 
      </v-list-item>
    </router-link>
      
    <router-link  v-show="user" :to="`/auth/profile/`">
      <v-list-item title="Profile" >Profile</v-list-item>
    </router-link>
    <router-link   v-show="user" :to="`/borrow/`">
      <v-list-item title="Borrow"  href="/borrow/" >Borrow</v-list-item>
    </router-link>

    <router-link   v-show="user" :to="`/borrow/meWait`">
      <v-list-item title="meWait" >MeWait</v-list-item>
    </router-link>
     
      <router-link   v-show="user" :to="`/borrow/meborrow`">
        <v-list-item title="MeBorrow"  >MeBorrow</v-list-item>
      </router-link>

      <router-link   v-show="user" :to="`/borrow/meReturn`">
        <v-list-item title="MeReturn"  >ReturnMe</v-list-item>
      </router-link>
      
      
      
      <!-- <v-list-item title="equipment"  href="/inv/equipment" >Equipment</v-list-item> -->
      <div v-if="$auth.loggedIn" >
        <div  v-if="user.u_stat === 'Admin'" >
          <router-link   v-show="user" :to="`/dashboard/`"> 
            <v-list-item  title="AdminDashboard">AdminDashboard</v-list-item>
          </router-link>
          <router-link   v-show="user" :to="`/dashboard/user`">
            <v-list-item  title="AdminUser">AdminUser</v-list-item>
          </router-link>
          <router-link   v-show="user" :to="`/dashboard/equipment`"> 
            <v-list-item  title="AdminEquipment">AdminEquipment</v-list-item>
          </router-link>
          <router-link   v-show="user" :to="`/dashboard/borrow`"> 
            <v-list-item  title="AdminBorrow">AdminBorrow</v-list-item>
          </router-link>
          <router-link   v-show="user" :to="`/dashboard/return`"> 
            <v-list-item  title="AdminReturn">AdminReturn</v-list-item>
          </router-link>
          <router-link   v-show="user" :to="`/dashboard/category`"> 
             <v-list-item  title="AdminCategory">AdminCategory</v-list-item>
          </router-link>
          <router-link   v-show="user" :to="`/dashboard/appBorrow`"> 
             <v-list-item  title="AdminCategory">AdminAppBorrow</v-list-item>
          </router-link>
      </div>
    </div>
      <!-- <v-list-item title="Regis"  href="/regis/" >logout</v-list-item> -->
    </v-navigation-drawer>
    
    <v-content  >
        
        <nuxt/>
      
    </v-content>
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
    user: {
      u_id: '',
      email: '',
      username: '',
      firstname: '',
      lastname: '',
      u_stat: '',
    },
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
  color: white;
}
</style>
