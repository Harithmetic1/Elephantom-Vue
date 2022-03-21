<template>
            <div class="hamburger-menu" @click="closeSideBar" >
                <div></div>
                <div></div>
                <div></div>
            </div>
</template>

<script>
    export default{
        name: "hamburger-menu",
        methods:{
            closeSideBar(){
                this.$emit("toggle-sidebar")
            }
        }
    }
</script>