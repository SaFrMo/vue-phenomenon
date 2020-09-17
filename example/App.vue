<template>
    <main class="example"></main>
</template>

<script>
/* eslint-disable */
import VuePhenomenon from '../src/VuePhenomenon'

export default {
    components: {
        'vue-phenomenon': VuePhenomenon,
    },
}
</script>

<style lang="scss">
.example {
    overflow-x: hidden;
    background: #c44;
    position: absolute;
    top: 0;
    right: 0;
    min-height: 100vh;
    left: 0;

    display: grid;
    grid-template-columns: repeat(2, 50vw);
    grid-gap: 5px;

    & > div {
        background: white;
        position: relative;
        font-size: 0;
        height: 50vh;

        &:last-child:nth-child(odd) {
            grid-column: span 2;
        }
    }

    .css {
        font-size: 16px;
    }
}
</style>
