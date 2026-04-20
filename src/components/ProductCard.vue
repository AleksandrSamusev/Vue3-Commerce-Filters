<script setup>
import Basket from './icons/Basket.vue';
import Heart from './icons/Heart.vue';
import { ref, computed } from 'vue';

const filled = ref(false);
const props = defineProps({
    product: {
        type: Object,
        required: true
    }
});
const discountPercentage = computed(() => {
    if (!props.product.discountPrice) return 0;
    const diff = props.product.price - props.product.discountPrice;
    return Math.round((diff / props.product.price) * 100);
});
</script>

<template>
    <div v-if="product" class="product-card">
        <div class="image-external-wrapper">
            <div class="image-wrapper">
                <span v-if="product.discountPrice" class="badge sale-badge">SALE</span>
                <!-- Show NEW only if it's NOT on sale and has the feature_new tag -->
                <span v-else-if="product.attributes.includes('feature_new')" class="badge new-badge">
                    NEW
                </span>
                <img :src="product?.url" :alt="product?.alt" srcset="">
            </div>
            <Heart @clicked="filled = !filled" :triggered="filled" class="add-to-favorite" />
            <Basket class="add-to-basket" />
        </div>
        <span>{{ product.name }}</span>

        <div class="price-row">
            <!-- Case A: Discounted -->
            <template v-if="product.discountPrice">
                <span class="original-price strike">$ {{ product.price.toFixed(2) }}</span>
                <span class="sale-price">$ {{ product.discountPrice.toFixed(2) }}</span>
                <span class="percentage">({{ discountPercentage }}% off)</span>
            </template>

            <!-- Case B: Normal -->
            <span v-else class="regular-price">$ {{ product.price.toFixed(2) }}</span>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.image-external-wrapper {
    position: relative;
}

.product-card {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    gap: 0.125rem;
    font-family: "Roboto";
    font-size: 20px;
}

.add-to-basket {
    position: absolute;
    bottom: -12px;
    right: 12px;
    cursor: pointer;
}

.add-to-favorite {
    position: absolute;
    top: 20px;
    right: 20px;
    width: 20px;
    height: auto;
    cursor: pointer;
}

.image-wrapper {
    position: relative;
    overflow: hidden;
    margin-bottom: 16px;
    cursor: pointer;
}

img {
    width: 400px;
}

.product-name {
    color: #040E45; // Dark blue tone from image
    font-weight: 500;
    margin-bottom: 4px;
}

.price-row {
    display: inline-flex;
    align-items: center;
    gap: 12px;
    font-size: 18px;
    font-weight: 600;
}

.strike {
    text-decoration: line-through;
    color: #040E45; // Original price is often dark blue/gray
    opacity: 0.6;
    font-weight: 400;
}

.sale-price,
.percentage {
    color: #d32f2f; // Red tone from your image
}

.percentage {
    font-weight: 400;
    font-size: 16px;
}

.badge {
    position: absolute; // 3. Moves freely inside the wrapper
    top: 15px; // 4. Distance from top
    left: -35px; // 5. Shift left so the rotation sits in the corner
    width: 120px; // 6. Needs enough width to cover the slanted area

    text-align: center;
    transform: rotate(-45deg); // 7. The slant!

    font-family: "Roboto", sans-serif;
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 1px;
    padding: 2px 0;
    z-index: 10;
}

.sale-badge {
    background-color: #CD0000; // Red
    opacity: 0.75;
    color: white;
}

.new-badge {
    background-color: #000000; // Gray
    opacity: 0.5;
    color: white;
}
</style>