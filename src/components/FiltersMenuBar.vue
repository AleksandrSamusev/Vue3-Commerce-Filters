<script setup>
import Chevron from './icons/Chevron.vue';
import { ref, computed } from 'vue';
import { onClickOutside } from '@vueuse/core';
const props = defineProps(['isOpen', 'showClear', 'allProdsCount', 'filteredProdsCount']);;
const emit = defineEmits(['toggle', 'clear-all', 'sort-change']);
const isVisible = ref(false);
const selectedSortingOption = ref('low-high')
const dropdownContainer = ref(null);
const dropdownTrigger = ref(null);
const sortingOptions = [
    { id: 'low-high', label: 'Low to High' },
    { id: 'high-low', label: 'High to Low' },
    { id: 'newest', label: 'Newest' },
    { id: 'a-z', label: 'A - Z' }
];
const handleSorting = () => {
    isVisible.value = false;
    emit('sort-change', selectedSortingOption.value);
};
onClickOutside(dropdownContainer, () => {
    isVisible.value = false;
}, { ignore: [dropdownTrigger] });
const currentLabel = computed(() => {
    return sortingOptions.find(o => o.id === selectedSortingOption.value)?.label || 'Low to High';
});
</script>

<template>
    <div class="filters-bar-wrapper">
        <div @click="$emit('toggle')" class="filter-by">
            <span class="label">filter By</span>
            <Chevron :class="{ 'rotate-180': isOpen }" class="chevron" />
        </div>

        <div class="sort-by">
            <Transition name="fade">
                <span v-if="showClear" @click="$emit('clear-all')" class="label clear-filters">
                    clear filters
                </span>
            </Transition>
            <span class="items-count">Items {{ props.filteredProdsCount }} of {{ props.allProdsCount }}</span>
            <div ref="dropdownTrigger" class="sort-chevron-wrapper" @click="isVisible = !isVisible;">
                <span class="label min-width-140">sort by: {{ currentLabel  }}</span>
                <Chevron :class="{ 'rotate-180': isVisible }" class="chevron" />
            </div>
            <div ref="dropdownContainer" v-show="isVisible" class="sorting-dropdown">
                <label :for="option.id" v-for="option in sortingOptions" class="sorting-option" :key="option.id">
                    <span>{{ option.label }}</span>
                    <input @change="handleSorting" v-model="selectedSortingOption" type="radio" name="option"
                        :value="option.id" :id="option.id" >
                </label>
            </div>
        </div>
    </div>
</template>




<style lang="scss" scoped>
.filters-bar-wrapper {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-top: 50px;
    color: #040E45;
    flex-wrap: wrap;
}

.label {
    font-family: "Bebas Neue", sans-serif;
    font-size: 24px;
    color: #040E45;
}

.filter-by {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    cursor: pointer;
}

.sort-by {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    flex-wrap: wrap;
    position: relative;
}

.items-count {
    font-family: "Roboto";
    font-size: 16px;
    min-width: 110px;
}

.chevron {
    width: 10px;
    height: auto;
    transition: transform 0.3s ease;
    cursor: pointer;
}

.rotate-180 {
    transform: rotate(180deg);
}

.clear-filters {
    //border: 1px solid #040E45;
    box-shadow: inset 0 0 0 1px #040E45;
    /* No layout shift at all */
    padding: 0rem 0.5rem;
    cursor: pointer;
}

.sorting-dropdown {
    font-family: Roboto;
    font-size: 16px;
    font-weight: bold;
    display: flex;
    gap: 0.75rem;
    flex-direction: column;
    justify-content: center;
    position: absolute;
    top: 100%;
    right: 0%;
    border: 1px solid #040E45;
    padding: 0.5rem;
    background-color: white;
    z-index: 99;
}

.sorting-option {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 2rem;
    cursor: pointer;
}

.sort-chevron-wrapper {
    display: flex;
    gap: 0.5rem;
    cursor: pointer;
}

.min-width-140 {
    min-width: 140px;
    text-align: right;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>