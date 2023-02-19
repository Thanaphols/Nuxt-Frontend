<template>
  
  <v-app  >
    <v-app-bar app class="primary" dark >
      <v-app-bar-nav-icon class="ml-2" @click="drawer = !drawer"></v-app-bar-nav-icon>
      
      <v-app-bar-title  text>
        <h5></h5>
        <router-link  style="text-decoration: none; color: inherit;"  to="/">ระบบยืม-คืนอุปกรณ์ 
      </router-link>
      </v-app-bar-title>
    
      <v-spacer/>
      <router-link  :to="`/auth/`">
        <svg-icon type="mdi" :path="path5"></svg-icon>
      
      </router-link>
        <v-btn  v-show="user" class="mr-2" plain  @click="logout()">ออกจากระบบ<span class="material-icons"><svg-icon type="mdi" :path="path"></svg-icon></span> </v-btn>
        <!-- <router-link  v-show="!user" :to="`/auth/`"><span class="material-icons"><svg-icon type="mdi" :path="path2"></svg-icon></span> </router-link> -->
        <v-checkbox 
      v-model="$vuetify.theme.dark"
      class="pt-5"
      color="indigo"
      off-icon="mdi-theme-light-dark"
      on-icon="mdi-theme-light-dark"
  ></v-checkbox>
     
    </v-app-bar>
 
    <v-navigation-drawer v-model="drawer"   app     >
      <router-link  v-show="!user" :to="`/auth/`">
      <v-list-item  v-show="!user"    title="Login" >
          <svg-icon type="mdi" class="mb-0" :path="path2"> </svg-icon> <span class="ml-2">เข้าสู่ระบบ</span>
      </v-list-item>
    </router-link>
      <router-link  v-show="!user" :to="`/auth/Regis/`">
      <v-list-item title="Regis"   >
        <span >สมัครใช้งาน</span> 
      </v-list-item>
    </router-link>

    <div v-if="$auth.loggedIn" >
        <div  v-if="user.u_stat === 'Approved' || user.u_stat === 'Admin' " >
          <router-link   v-show="user" :to="`/auth/dashboard`"> 
            <v-list-item  title="AdminDashboard"><svg-icon type="mdi" :path="path3"></svg-icon> ยืม-คืน</v-list-item>
          </router-link>
        </div>
    </div>
      
    <router-link  v-show="user" :to="`/auth/profile/`">
      <v-list-item title="Profile" ><svg-icon type="mdi" :path="path4"></svg-icon> โปรไฟล์</v-list-item>
    </router-link>
    <router-link   v-show="user" :to="`/borrow/`">
      <v-list-item title="Borrow"  href="/borrow/" >ยืมอุปกรณ์</v-list-item>
    </router-link>

    <!-- <router-link   v-show="user" :to="`/borrow/meWait`">
      <v-list-item title="meWait" >คำร้องขอยืม</v-list-item>
    </router-link>
     
      <router-link   v-show="user" :to="`/borrow/meborrow`">
        <v-list-item title="MeBorrow"  >อุปกรณ์ที่กำลังถูกยืม</v-list-item>
      </router-link>

      <router-link   v-show="user" :to="`/borrow/meReturn`">
        <v-list-item title="MeReturn"  >อุปกรณ์ที่คืนแล้ว</v-list-item>
      </router-link> -->
      
     
      
      <!-- <v-list-item title="equipment"  href="/inv/equipment" >Equipment</v-list-item> -->
      <div v-if="$auth.loggedIn" >
        <div  v-if="user.u_stat === 'Admin'" >
          <router-link   v-show="user" :to="`/dashboard/`"> 
            <v-list-item  title="AdminDashboard"><svg-icon type="mdi" :path="path3"></svg-icon> Dashboard</v-list-item>
          </router-link>
          <router-link   v-show="user" :to="`/dashboard/user`">
            <v-list-item  title="AdminUser">ข้อมูลผู้ใช้งาน</v-list-item>
          </router-link>
          <router-link   v-show="user" :to="`/dashboard/equipment`"> 
            <v-list-item  title="AdminEquipment">ข้อมูลอุปกรณ์</v-list-item>
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
    
    <v-main>
      <v-container grid-list-xs>
          <nuxt/>
        </v-container>
    </v-main>
  </v-app>
</template>

<script>
import SvgIcon from '@jamescoyle/vue-icon';
import { mdiLogoutVariant,mdiAccount,mdiViewDashboardOutline,mdiAccountEdit,
  mdiHomeCircleOutline 
        } from '@mdi/js';
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
    path3: mdiViewDashboardOutline,
    path4: mdiAccountEdit,
    path5: mdiHomeCircleOutline,
      drawer: true,
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
/* width */
::-webkit-scrollbar {
    width: 5px;
  }

  /* Track */
  ::-webkit-scrollbar-track {
    background: #000000;
  }

  /* Handle */
  ::-webkit-scrollbar-thumb {
    background: #555;
  }

  /* Handle on hover */
  ::-webkit-scrollbar-thumb:hover {
    background: #555;
  }

</style>
