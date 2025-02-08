<template>
  <div style="margin: 20px;">
    <div style="display: flex; align-items: center; margin-bottom: 20px;">
      <Button icon="pi pi-arrow-left" @click="goBack" severity="secondary" style="margin-right: 10px;" />
      <h1>Product Details</h1>
    </div>
    <p><strong>Product ID:</strong> {{ productId.code ? productId.code : productId.data.code
      }}
    </p>
    <strong>Product Data</strong>
    <div v-if="productId.code">
      <div v-for="product in productId" :key="product.code">
    {{ product }}
  </div>
</div>
<div v-else>
 
  <div v-for="product in productId.data" :key="product.code">
    {{ product }}
  </div>
</div>


    <div class="card">
      <Tabs value="/dashboard">
        <TabList>
          <Tab v-for="tab in items" :key="tab.label" :value="tab.route">
            <div class="custom-flex" @click="navigateToTab(tab.route)">
              <i :class="tab.icon" />
              <span>{{ tab.label }}</span>
            </div>
          </Tab>
        </TabList>
        <TabPanels>
          <TabPanel v-for="tab in items" :key="tab.label" :header="tab.label">
            <div>
              <!-- Displaying the content based on productId data -->
              <p>{{ productId.data }}</p>
            </div>
          </TabPanel>
        </TabPanels>
      </Tabs>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Tabs from 'primevue/tabs';
import TabList from 'primevue/tablist';
import Tab from 'primevue/tab';
import TabPanels from 'primevue/tabpanels';
import TabPanel from 'primevue/tabpanel';
import { defineEmits, defineProps } from 'vue';

const props = defineProps(['productId']); // Receive productId as a prop
const emit = defineEmits(); // Define emits for the component

const items = ref([
  { route: '/dashboard', label: 'Dashboard', icon: 'pi pi-home' },
  { route: '/transactions', label: 'Transactions', icon: 'pi pi-chart-line' },
  { route: '/products', label: 'Products', icon: 'pi pi-list' },
  { route: '/messages', label: 'Messages', icon: 'pi pi-inbox' }
]);

const goBack = () => {
  emit('back'); // Emit a 'back' event to the parent component
};

const navigateToTab = (route) => {
  // Use Vue Router to navigate programmatically
  this.$router.push(route);
};
</script>

<style scoped>
.custom-flex {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  /* 8px */
  color: inherit;
}
</style>
