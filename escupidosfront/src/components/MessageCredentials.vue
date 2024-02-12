<template>
    <div class="row">
        <img src="" alt="icono">
        <h1>{{ props.parametro }}</h1>
    </div>
</template>

<script lang="ts" setup>
    import { defineProps } from 'vue';
    const props = defineProps({parametro: String});
</script>

<style scoped>
    h1{
        margin-left: 2rem;
    }
    .row{
        display: flex;
        flex-direction: row;
        align-items: center;
        padding: 40px;
    }
</style>