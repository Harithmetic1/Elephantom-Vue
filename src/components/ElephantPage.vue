<template>
    <div className="elephant-container" >
                <breadcrumb @toggle-sidebar="toggleSidebar" :elephantName="elephantDetails.name" :showTable="showTable" />

                <div className="elephant-content-area">
                    <div className="elephant-image">
                        <img :src="elephantDetails.image" :alt="elephantDetails.name" />
                    </div>
                    <div className="elephant-details">
                        <h1>{{elephantDetails.name}}</h1>
                        <p>{{elephantDetails.sex}}</p>
                    </div>
                    <div className="elephant-notes">
                        <p>
                            {{elephantDetails.note}}
                        </p>
                    </div>
                </div>
            </div>
</template>

<script>

    import Breadcrumb from "./Breadcrumb.vue"

    export default{
        name: "elephant-page",
        components:{
            Breadcrumb
        },
        props:{
            elephantDetails:{
                type: Object,
                required: true
            },
            showTable:{
                type: Boolean,
            }
        },
        methods:{
            toggleSidebar(){
                this.$emit("toggle-sidebar")
            },
        }
    }
</script>