<template>
    <div v-if="content === 'dataload'" class="centered-container">
        <newtable @product-selected="showProductDetail" />
    </div>
    <div v-else>
        <product-detail :productId="selectedProductId"  @back="goBack" />
    </div>
</template>

<script setup>
import { ref } from 'vue';
import newtable from '@/components/newTable.vue'; // Ensure the path is correct
import ProductDetail from '@/components/ProductDetails.vue'; // Ensure the path is correct

const content = ref('dataload');
const selectedProductId = ref(null); // To store the selected product ID

const updateContent = (message) => {
    content.value = message;
};

const showProductDetail = (productId) => {
    console.log('Selected Product ID:', productId); // Debugging step
    selectedProductId.value = productId;
    content.value = 'productDetail';
};


const goBack = () => {
    content.value = 'dataload'; // Change content back to show newtable
    selectedProductId.value = null; // Clear the selected product ID if needed
};
</script>

<style scoped>
.centered-container {
    
}
</style>