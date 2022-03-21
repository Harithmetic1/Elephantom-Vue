<template>
    <section class="rhs">
                <div class="rhs-container">
                    <breadcrumb @toggle-sidebar="toggleSidebar" :showTable="showTable"></breadcrumb>
                    <section class="table-area">
                        <div class="table-area-container">

                        <div class="table-title">
                            All Elephants
                        </div>
                        <table>
                            <thead>
                                <tr class="table-head">
                                    <th>S/N</th>
                                    <th>Name</th>
                                    <th>Species</th>
                                    <th>Sex</th>
                                    <th>Affiliation</th>
                                    <th>DOB</th>  
                                </tr>
                            </thead>
                            <tbody id="elephant-table">
                               <table-row @populate-elephant-page="emitSelected" v-for="(elephant, index) in currentPage" :elephant="elephant" :key="index"></table-row>
                            </tbody>
                        </table>
                    </div>
                    <div class="pagination-container">
                        <div class="page-btn-container">
                            <div class="pagination-tag">
                                Page {{pageNumber}} of {{totalPages}}
                            </div>
                            <div>
                                <!-- <button>
                                    {{"<"}}
                                </button> -->
                                    <button v-for="page in totalPages" :key="page" :value="page" @click="choosePage" >{{page}}</button>
                                <!-- <button>
                                    {{">"}}
                                </button> -->
                            </div>
                        </div>
                        </div>
                    </section>
                </div>
            </section>
</template>

<script>

    import tableRow from "./TableRow.vue"
    import Breadcrumb from "./Breadcrumb.vue"

    export default {
        name: "table-body",
        components: {
            tableRow,
            Breadcrumb
        },
        props:{
            showTable:{
                type: Boolean
            }
        },
        data(){
            return{
                elephantData: [],
                current: 1,
                rows: 5,
                totalPages: 0,
                trimStart: 0,
                trimEnd: 5,
            }
        },
        async mounted() {
            var elephant = await fetch("https://acumen-elephantom.herokuapp.com/elephants/asian")
            elephant = await elephant.json()
            this.elephantData = elephant.data.sort((a, b) => (a.index > b.index) ? 1 : -1)
            console.log(this.elephantData);
            this.setPagination(elephant)
        },
        methods: {
            setPagination(arrayData){
                // this.currentPage = arrayData.data.slice(this.trimStart, this.trimEnd)
                this.totalPages = Math.ceil(arrayData.data.length / this.rows)
            },
            choosePage(e){
                this.current = e.target.value
                this.trimStart = (e.target.value - 1) * this.rows
                this.trimEnd = this.trimStart + this.rows
                console.log(this.trimStart, this.trimEnd);
            },
            emitSelected(e){
                this.$emit("elephant-chosen", e)
            },
            toggleSidebar(){
                this.$emit("toggle-sidebar")
            },
        },
        computed: {
            totalData() {
                if(this.elephantData){
                    return this.elephantData.length
                }
                return null
            },
            currentPage() {
                return this.elephantData.slice(this.trimStart, this.trimEnd)
            },
            pageNumber(){
                return this.current
            }
        }
    }
</script>