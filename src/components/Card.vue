<template lang="">

        <button  v-bind:style="{ backgroundImage: 'url(' + img + ')' }"></button>


</template>
<script>
export default {
    props: ['img']
}
</script>
<style scoped>
button {
    background-color: black;
    width: 21%;
    aspect-ratio: 1/1;
    padding: 0;
    margin: 0;
    border: none;
    background-size: contain;
    color: red;
}

@media (max-width: 800px) {
    button {
        width: 34%;
    }
}
</style>