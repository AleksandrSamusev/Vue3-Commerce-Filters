<script setup>
import ProductCard from './ProductCard.vue';

defineProps(['prods']);
</script>


<template>
    <div class="products-wrapper">
        <ProductCard v-for="rec in prods" :key="rec.id" :product="rec" />
    </div>
</template>

<style lang="scss" scoped>
.products-wrapper {
    margin-top: 2rem;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    flex-wrap: wrap;
    gap: 3.3rem;
    min-height: 400px;
}
</style>