<script setup>
    import { ref } from 'vue';

    const props = defineProps(['value', 'index', 'move'])
</script>

<template>
    <button @click="move(index)">{{value}}</button>
</template>

<style lang="scss" scoped>

    button {
        margin: 0;
        height: 100px;
        width: 100px;
        font-size: 32px;
        border: 1px grey solid;
        &:hover{
            background: rgb(220,220,220);
        }
    }

</style>


