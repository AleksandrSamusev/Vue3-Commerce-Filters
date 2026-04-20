<script setup>
import { ref, computed } from 'vue';
import allProducts from '@/seeder.js'; // Import your refactored seeder
import Header from '@/components/Header.vue'
import Banner from '@/components/Banner.vue';
import Hero from './components/Hero.vue';
import FiltersMenuBar from './components/FiltersMenuBar.vue';
import FiltersCategoriesBar from './components/FiltersCategoriesBar.vue';
import ProductsList from './components/ProductsList.vue';
import CategoryOptionsList from './components/CategoryOptionsList.vue';

const isMenuOpen = ref(false);
const selectedId = ref(null);
const products = ref(allProducts);
const currentSort = ref('low-high');
// Store selections as: { categoryId: [optionId, optionId] }
const selections = ref({});

const clearCategoryFilters = (catId) => {
  selections.value[catId] = [];
  selections.value = { ...selections.value };
  selectedId.value = null;
};

const handleOptionToggle = (catId, optId) => {
  if (!selections.value[catId]) {
    selections.value[catId] = [];
  }
  const index = selections.value[catId].indexOf(optId);
  if (index > -1) {

    selections.value[catId].splice(index, 1); // Remove if exists
    // If we just removed the last option, close the drawer
    if (selections.value[catId].length === 0) {
      selectedId.value = null;
    }
  } else {
    selections.value[catId].push(optId); // Add if new
  }
  // FORCE REACTIVITY: Re-assign the object so Vue "sees" the internal array change
  // This ensures your badges in the CategoriesBar update instantly
  selections.value = { ...selections.value };
};

const resetAllFilters = () => {
  selections.value = {};    // 1. Clears all badges and checkboxes
  selectedId.value = null;   // 2. Removes border and hides the options bar
};

const hasAnyFilters = computed(() => {
  return Object.values(selections.value).some(arr => arr.length > 0);
});

// const filteredProducts = computed(() => {
//   // 1. If no filters are active, return everything
//   if (!hasAnyFilters.value) {
//     return products.value;
//   }
//   return products.value.filter(product => {
//     const virtualAttributes = [...product.attributes];
//     const effectivePrice = product.discountPrice ?? product.price;
//     // Add the correct Price Range tag
//     if (effectivePrice < 10) virtualAttributes.push('price_less_than_ten');
//     else if (effectivePrice <= 50) virtualAttributes.push('price_ten_to_fifty');
//     else virtualAttributes.push('price_above_fifty')

//     // Add the correct Discount status tag
//     const hasDiscount = product.discountPrice !== null && product.discountPrice < product.price;
//     virtualAttributes.push(hasDiscount ? 'discount_with_discount' : 'discount_without_discount');

//     // 2. We must pass the check for EVERY active category
//     return Object.entries(selections.value).every(([catId, selectedOptions]) => {
//       // If this category is empty, it doesn't filter the product
//       if (selectedOptions.length === 0) return true;

//       // Check if the product has at least one of the selected option IDs
//       return selectedOptions.some(optId => virtualAttributes.includes(optId));
//     });
//   });
// });
const filteredProducts = computed(() => {
  // 1. Create a copy to avoid mutating the master list
  let result = [...products.value];

  // 2. Apply Filtering
  if (hasAnyFilters.value) {
    result = result.filter(product => {
      const virtualAttributes = [...product.attributes];
      const effectivePrice = product.discountPrice ?? product.price;

      // Add Price Brackets
      if (effectivePrice < 10) virtualAttributes.push('price_less_than_ten');
      else if (effectivePrice <= 50) virtualAttributes.push('price_ten_to_fifty');
      else virtualAttributes.push('price_above_fifty');

      // Add Discount Tags
      const hasDiscount = product.discountPrice !== null && product.discountPrice < product.price;
      virtualAttributes.push(hasDiscount ? 'discount_with_discount' : 'discount_without_discount');

      // Check selections
      return Object.entries(selections.value).every(([catId, selectedOptions]) => {
        if (selectedOptions.length === 0) return true;
        return selectedOptions.some(optId => virtualAttributes.includes(optId));
      });
    });
  }

  // 3. Apply Sorting
  // We use .sort() on the result array (which is already a copy)
  return result.sort((a, b) => {
    // Helper for Price Sorting (always uses the sale price if available)
    const priceA = a.discountPrice ?? a.price;
    const priceB = b.discountPrice ?? b.price;

    if (currentSort.value === 'low-high') {
      return priceA - priceB;
    }

    if (currentSort.value === 'high-low') {
      return priceB - priceA;
    }

    if (currentSort.value === 'a-z') {
      return a.name.localeCompare(b.name);
    }

    if (currentSort.value === 'newest') {
      // Sort by Date: later date minus earlier date
      return new Date(b.createdAt) - new Date(a.createdAt);
    }

    return 0; // Default: no change
  });
});
</script>

<template>
  <Header />
  <Banner />
  <div class="content-wrapper">
    <Hero />
  </div>
  <div class="gray-line"></div>
  <div class="content-wrapper">
    <FiltersMenuBar :isOpen="isMenuOpen" @toggle="isMenuOpen =
      !isMenuOpen" :showClear="hasAnyFilters" @clear-all="resetAllFilters" :all-prods-count="products.length"
      :filtered-prods-count="filteredProducts.length" @sort-change="currentSort = $event" />
    <FiltersCategoriesBar :catId="selectedId" :allSelections="selections" @selected-category="selectedId = $event"
      :isVisible="isMenuOpen" />
    <CategoryOptionsList v-if="selectedId && isMenuOpen" :catId="selectedId" @toggle-option="handleOptionToggle"
      :allSelections="selections" @clear-all="resetAllFilters" @close="selectedId = null"
      @clear-category="clearCategoryFilters" />
    <ProductsList :prods="filteredProducts" />
  </div>
</template>

<style scoped>
.content-wrapper {
  padding: 0 7%;
}

.isVisible {
  display: none;
}

.gray-line {
  height: 1px;
  width: 100%;
  background-color: #E9E9E9;
}
</style>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
}



body,
h1,
h2,
h3,
h4,
p,
figure,
blockquote,
dl,
dd {
  margin: 0;
}

html,
body {
  height: 100%;
}

body {
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}

input,
button,
textarea,
select {
  font: inherit;
}

img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
}

#app {
  user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
</style>