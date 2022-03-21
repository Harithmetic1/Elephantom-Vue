<template>
    <div class="table-area-header">
                <hamburger @toggle-sidebar="toggleSidebar" />
                <ul id="breadcrumb">
                    <li><p>Acumen Digital Interview Task / Elephantom</p></li>
                        <li class="crumb" v-if="!showTableProp"><p>{{elephantName}}</p></li>
                </ul>
            </div>
</template>

<script>
    import Hamburger from "./Hamburger.vue"

    export default{
        name: "bread-crumb",
        components:{
            Hamburger
        },
        props:{
            showTable:{
                type: Boolean
            },
            elephantName: {
                type: String
            }
        },
        methods:{
            toggleSidebar(){
                this.$emit("toggle-sidebar")
            },
        },
        computed: {
            showTableProp(){
                return this.showTable
            }
        }
    }
</script>