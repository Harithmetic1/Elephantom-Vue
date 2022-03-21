<template>
    <tr @click="populateElephantPage">
        <td>{{elephant.index}}</td>
        <td>{{elephant.name}}</td>
        <td>{{elephant.species}}</td>
        <td>{{elephant.sex}}</td>
        <td>{{elephant.affiliation}}</td>
        <td>{{elephant.dob}}</td>
    </tr>
</template>

<script>

    export default{
        name: "tableRow",
        props: {
            elephant: {
                type: Object,
                require: true
            }
        },
        methods: {
            populateElephantPage(){
                this.$emit("populate-elephant-page", this.elephant)
                console.log(`${this.elephant.name} was clicked`);
            }
        }
    }
    

</script>