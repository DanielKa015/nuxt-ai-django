<script setup>
const searchQuery = ref("star")
const resultQuery = ref( null )

const searchData = async() => {
    await useFetch('https://images-api.nasa.gov/search?q=' + searchQuery.value).then((response) => {
        resultQuery.value = response.data
}) 
}
    
onBeforeMount(() => {
    searchData()
});
</script>
<template>
    <div>
        <input type="text" v-model="searchQuery">
        <button @click="searchData">Wyszukaj</button>
    </div>

 
</template>
<style scoped lang="scss">
h3 {
    span {
        color: green
    }
    
}
</style>