<template>
  <div class="container">
    <side-bar @toggle-sidebar="toggleSidebarMethod" v-if="showSidebarHandler" :showTable="showTableHandler" @set-show-table="hideElephantPage"></side-bar>
    <table-body-vue @elephant-chosen="showElephantPage" @toggle-sidebar="toggleSidebarMethod" v-if="showTable" :showTable="showTableHandler"></table-body-vue>
    <elephant-page @toggle-sidebar="toggleSidebarMethod" :elephantDetails="elephantPageDetails" :showTable="showTableHandler" v-if="!showTable" />
  </div>
</template>

<script>
import SideBar from "./components/Sidebar.vue"
import TableBodyVue from './components/TableBody.vue'
import ElephantPage from "./components/ElephantPage.vue"

export default {
  name: 'App',
  components: {
    SideBar,
    TableBodyVue,
    ElephantPage
  },
  data(){
    return{
      showTable: true,
      elephantPageDetails: [],
      showSidebar: true
    }
  },
  methods:{
    showElephantPage(data){
      this.elephantPageDetails = data
      this.showTable = false
      console.log(data);
    },
    hideElephantPage(){
      this.showTable = true
    },
    toggleSidebarMethod(){
      this.showSidebar = !this.showSidebar
    }
  },
  computed:{
    elephantPageHandler(){
      return this.elephantPageDetails
    },
    showTableHandler(){
      return this.showTable
    },
    showSidebarHandler(){
      return this.showSidebar
    }
  }
}
</script>


