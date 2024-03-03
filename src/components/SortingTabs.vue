<script>
export default {
    data() {
        return {
            sort: 'ASC'
        }
    },
    methods: {
        changeSort( sort ) {
            this.sort = sort
            console.log( sort );
        }
    },
}
</script>

<template>
    <div class="">
        <button @click="changeSort('ASC')" data-sort="ASC" class="sorting--active">Сначала новые</button>
        <button @click="changeSort('DESC')" data-sort="DESC" >Сначала старые</button>
    </div>
</template>

<style scoped>

</style>