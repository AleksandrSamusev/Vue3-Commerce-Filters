<script setup>
import categories from '@/filterCategories';

const props = defineProps(['isVisible', 'catId', 'allSelections']);
const emit = defineEmits(['selectedCategory']);
const handleCategorySelection = (id) => {
    const toggleId = id === props.catId ? null : id;
    emit('selectedCategory', toggleId);
}
</script>
<template>
    <div v-show="isVisible" class="categories-wrapper">
        <div @click="handleCategorySelection(category.id)" :class="{ 'selected': catId === category.id }"
            class="category" v-for="category in categories" :key="category.id">
            <span>{{ category.name }}</span>
            <Transition name="fade">
                <span v-if="allSelections?.[category.id]?.length" class="badge">
                    {{ allSelections[category.id].length }}
                </span>
            </Transition>
            <span>+</span>
        </div>
    </div>
</template>
<style lang="scss" scoped>
.categories-wrapper {
    margin-top: 30px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    flex-wrap: wrap;
    gap: 1rem;
}

.category {
    display: flex;
    align-items: center;
    padding: 1rem;
    justify-content: space-between;
    width: 200px;
    height: 60px;
    border: 1px solid #E9E9E9;
    font-family: "Roboto";
    font-weight: 600;
    font-size: 16px;
    color: #040E45;
    cursor: pointer;
}

.selected {   
    border: 2px solid #040E45;
}

.badge {
    text-align: center;
    width: 20px;
    height: 20px;
    border-radius: 10px;
    background-color: #040E45;
    color: white;
    font-family: "Roboto";
    font-size: 14px;
    font-weight: 600;
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