<template>
    <div>
        <h3> Select your favorite genre :</h3>

        <div v-for="(genre, index) in genres">
            <input type="checkbox" v-model="genres[index]"> {{index}}
        </div>
    </div>
</template>

<script>
    export default {
        name: "GenreChoose",
        props: {
            genres: {
                type: Object,
                default: {}
            }
        }
    }
</script>

<style scoped>

</style>
