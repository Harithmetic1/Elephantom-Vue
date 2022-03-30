<template>
    <section class="rhs">
                <div class="rhs-container">
                    <breadcrumb @toggle-sidebar="toggleSidebar" :showTable="showTable"></breadcrumb>
                    <section v-if="totalData > 0" class="table-area">
                        <div class="table-area-container">

                        <div class="table-title">
                            All Elephants
                        </div>
                        <table >
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
                                <button :class="{disabled: pageLeftEndHandler}" @click="decreasePagination">
                                    {{"<"}}
                                </button>
                                    <button v-for="page in pageBtns" :key="page" :value="page" @click="choosePage" :class="[page == pageNumber ? 'activePageBtn' : null]" >{{page}}</button>
                                <button :class="{disabled: pageRightEndHandler}" @click="addPagination">
                                    {{">"}}
                                </button>
                            </div>
                        </div>
                        </div>
                    </section>
                    <div v-else-if="totalData <= 0" class="loader">
                            <!-- <img src="../assets/loader.gif" alt="Loader"> -->
                            <strong>Loading...</strong>
                    </div>
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
                window: 2,
                maxLeft: 1,
                maxRight: 2,
                pageLeftEnd: false,
                pageRightEnd: false,
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
            addPagination(){
                if(this.maxRight >= this.totalPages){
                    this.pageRightEnd = true
                    this.pageEnd = true
                    this.maxLeft = this.totalPages - (this.window - 1)
                    this.maxRight = this.totalPages
                }else{
                    this.pageRightEnd = false
                    this.maxRight += 1
                    this.maxLeft += 1
                }   
            },
            decreasePagination(){
                if(this.maxLeft <= 1){
                    this.pageLeftEnd = true
                    this.maxLeft = 1
                    this.maxRight = 2
                }else{
                    this.pageLeftEnd = false
                    this.maxRight -= 1
                    this.maxLeft -= 1
                }
                console.log(this.maxRightHandler);
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
            },
            maxLeftHandler(){
                return this.maxLeft
            },
            maxRightHandler(){
                    // if(this.maxLeft <= 1){
                    // return Number(this.current) + Math.floor(this.window / 2)
                    // }
                    return this.maxRight
            },
            pageBtns(){
                return [this.maxLeft, this.maxRightHandler]
            },
            pageLeftEndHandler(){
                if(this.maxLeft < 2){
                    return true
                }
                return false
            },
            pageRightEndHandler(){
                if(this.maxRight >= this.totalPages){
                    return true
                }
                return false
            },
        }
    }
</script>