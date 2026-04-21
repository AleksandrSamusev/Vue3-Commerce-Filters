<script setup>
import categories from '@/filterCategories'
import ClearXIcon from './icons/ClearXIcon.vue';
import { computed } from 'vue';

const props = defineProps(['catId', 'allSelections']);
const emit = defineEmits(['toggle-option', 'close', 'clear-all', 'clear-category']);

const selectedCat = computed(() => {    
    return categories.find(cat => cat.id == props.catId);
});

</script>
<template>
    <div class="options-wrapper">
        <div class="options-list">
            <label class="option" v-for="option in selectedCat?.options" :key="option.id">
                <input :checked="allSelections?.[catId]?.includes(option.id)"
                    @change="$emit('toggle-option', catId, option.id)" type="checkbox" :name="option.name"
                    :id="option.id" class="option-checkbox">
                <span class="option-name">{{ option.name }}</span>
            </label>
            <ClearXIcon class="close-btn" @click="$emit('clear-category', catId)" />
        </div>
    </div>
</template>








<style lang="scss" scoped>
.options-wrapper {
    font-family: "Roboto";
    font-size: 16px;
    font-weight: 400;
    color: #040E45;
    margin-top: 2rem;
    margin-bottom: 4rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.option {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    cursor: pointer;
}

.options-list {
    display: flex;
    gap: 3rem;
    align-items: center;
    justify-content: flex-start;
    flex-wrap: wrap;
}

.close-btn {
    margin-left: 3rem;
    width: 16px;
    height: auto;
    cursor: pointer;
}

.option-name {
    font-family: Roboto;
    font-size: 16px;
    font-weight: bold;
}

.option-checkbox {
    width: 20px;
    height: 20px;
    cursor: pointer;
    margin: 0;
}
</style>