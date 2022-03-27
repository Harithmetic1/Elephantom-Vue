<template>
    <section class="lhs">
                <section class="sidebar">
                    <hamburger @toggle-sidebar="toggleSidebar" />
                        <div class="sidebar-img">
                            <img src="../images/Sidebar-logo.png" alt="Elephant head" />
                        </div>
                        <div v-if="showTable" class="sidebar-links">
                            <div class="slinks active" id="home-cursor" @click="setShowTable">
                                <p class="home-link" >Home</p>
                            </div>
                            <div class="slinks" >
                                <p class="home-link">Elephant</p>
                            </div>
                        </div>
                        <div v-if="!showTable" class="sidebar-links">
                            <div class="slinks" id="home-cursor" @click="setShowTable">
                                <p class="home-link">Home</p>
                            </div>
                            <div class="slinks active" >
                                <p class="home-link">Elephant</p>
                            </div>
                        </div>
                    </section>
        </section>
</template>

<script>
    // import sideBarLogo from "../images/Sidebar-logo.png";
    import Hamburger from "./Hamburger.vue"

    export default{
        name: "SideBar",
        components:{
            Hamburger
        },
        props:{
            showTable:{
                type: Boolean
            }
        },
        data(){
            return{
                homeStyle: "slinks active",
                elephantStyle: "slinks"
            }
        },
        methods:{
            setShowTable(){
                this.$emit("set-show-table")
            },
            toggleSidebar(){
                this.$emit("toggle-sidebar")
            },
            setStyleHandler(){
                if (this.showTable){
                    this.homeStyle = "slinks"
                    this.elephantStyle = "slinks active"
                }else {
                    this.homeStyle = "slinks active"
                    this.elephantStyle = "slinks"
                }
            }
        },
        computed: {
            homeStyleHandler(){
                return this.homeStyle
            },
            elephantStyleHandler(){
                return this.elephantStyle
            }
        }
    }

</script>