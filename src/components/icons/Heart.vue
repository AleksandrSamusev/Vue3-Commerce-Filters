<script setup>
defineEmits(['clicked']);
const props = defineProps({ triggered: Boolean })
</script>
<template>
    <svg :class="{ 'filled': props.triggered }" @click="$emit('clicked')" width="17" height="16" viewBox="0 0 17 16"
        fill="none" xmlns="http://www.w3.org/2000/svg">
        <path
            d="M4.28024 0.502347C6.31538 0.401578 8.49429 3.57905 8.49429 3.57905C8.49429 3.57905 10.6732 0.401578 12.7083 0.502347C14.426 0.587396 15.7562 1.34905 16.3204 3.02148C16.6334 3.94917 16.502 4.58311 16.3204 5.54061C16.1387 6.49811 8.49429 14.4981 8.49429 14.4981C8.49429 14.4981 0.785578 6.51736 0.668193 5.54061C0.550808 4.56385 0.355211 3.94917 0.668193 3.02148C1.23243 1.34905 2.56258 0.587396 4.28024 0.502347Z"
            stroke="black" stroke-opacity="0.8" />
    </svg>
</template>
<style lang="scss" scoped>
@keyframes heartPop {
    0% {
        transform: scale(1);
    }

    50% {
        transform: scale(1.35);
    }

    100% {
        transform: scale(1);
    }
}

.filled {
    animation: heartPop 0.3s ease-out;
    fill: #000000;
}
</style>