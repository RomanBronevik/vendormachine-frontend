<template>
<div class="dashboard__grid-container">
  <header class="dashboard__header border-b border-grey-200 flex flex-row  px-6 items-center">
    <i class="far fa-bell mr-4 text-xl  ml-auto"></i>
    <div>
      <span v-click-outside="hideUserMenu" @click="showUserMenu = !showUserMenu" class="font-semibold relative cursor-pointer select-none">{{ user.name }}</span>
      <div v-if="showUserMenu" class="mt-2 py-2 w-48 bg-white border border-grey-300 rounded-lg shadow-xl absolute mt-2 right-0 mr-2">
        <span @click="logout()" class="block px-4 py-2 text-gray-800 hover:bg-indigo-500 cursor-pointer">Sign out</span>
      </div>
    </div>
  </header>
  <aside class="dashboard__sidenav border-r border-grey-200">
    <div class="dashboard__sidenav-logo">
      <i class="fa fa-store text-bluegem-500"></i> <span class="font-bold text-bluegem-500">vendor</span><span class="font-normal">machine</span>
    </div>
    <div class="dashboard__sidenav-content">
      <ul>
        <li class="dashboard__sidenav-menu-item dashboard__sidenav-menu-item--selected"><i class="fa fa-store dashboard__sidenav-menu-item-icon"></i> Vendors</li>
        <li class="dashboard__sidenav-menu-item"><i class="fa fa-tools dashboard__sidenav-menu-item-icon"></i> Settings</li>
      </ul>
    </div>
  </aside>
  <main class="dashboard__main">
    <div class="dashboard__main-content">
      <nuxt/>
    </div>
  </main>
</div>
</template>

<script>
  import Sidebar from '@/components/Page/Sidebar';
  import AddVendorModal from '@/components/Modals/AddVendorAccount';
  import ClickOutside from 'vue-click-outside'
  import { mapActions, mapGetters } from 'vuex';

  export default {
    components: { AddVendorModal, Sidebar },

    data(){
      return {
        showUserMenu: false
      }
    },

    directives: {
      ClickOutside
    },

    created() {
      if (this.$auth.loggedIn && ! this.activeVendor && this.user.vendors.data[0]) {        
        this.setActiveVendor(this.user.vendors.data[0].id)
      }
    },

    computed: {
      ...mapGetters({activeVendor: 'vendors/activeVendor'}),
      user() {
        if (this.$auth.loggedIn) {
          return this.$auth.user;
        }
      }
    },

    methods: {
      ...mapActions({setActiveVendor: 'vendors/setActive'}),
      
      hideUserMenu() {
        this.showUserMenu = false;
      },

      logout() {
        this.$auth.logout().then(() => {
          this.$toast.show('You have logged out')
          this.$nuxt.$emit('logout')
          this.$router.push('/login')
        })
      },
    },
  }
</script>