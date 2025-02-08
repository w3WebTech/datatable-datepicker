<template>
    <div class="p-5 mt-10">
        <div>
            <div style="display: flex; justify-content: space-between; align-items: center;margin-inline: 20px;">
                <div>
                    <IconField style="padding-left: 10px;">
                        <InputIcon>
                            <i class="pi pi-search" style="margin-left: 6px;" />
                        </InputIcon>
                        <InputText v-model="searchQuery" placeholder="Search" @input="filterProducts" />
                    </IconField>
                </div>
                <div>
                    <ConfirmPopup group="headless">
                        <template #container="{ message, acceptCallback, rejectCallback }">
                            <div class="px-4 py-2"> <i class="pi pi-clock p-1 my-2"
                                    style="font-size: 1rem"></i>Frequently used time period</div>
                            <div>

                                <div class="card  px-2">
                                    <div class="flex px-2 py-2 gap-2">
                                        <Button label="Week" size="small" class="flex-button" severity="secondary"
                                            @click="setDateFilter('week')" />
                                        <Button label="15 days" size="small" class="flex-button" severity="secondary"
                                            @click="setDateFilter('15days')" />
                                        <Button label="Month" size="small" class="flex-button" severity="secondary"
                                            @click="setDateFilter('month')" />
                                    </div>
                                    <div class="flex px-2 py-2 gap-2">
                                        <Button label="3 months" size="small" class="flex-button" severity="secondary"
                                            @click="setDateFilter('3months')" />
                                        <Button label="Year" size="small" class="flex-button" severity="secondary"
                                            @click="setDateFilter('year')" />
                                        <Button label="Last 6 Months" size="small" class="flex-button"
                                            severity="secondary" @click="setDateFilter('6months')" />
                                    </div>
                                    <div class="flex px-2 py-2 gap-2">
                                        <Button label="Year till date" size="small" class="flex-button"
                                            severity="secondary" @click="setDateFilter('ytd')" />
                                    </div>
                                </div>

                                <div class="px-4 py-2"> <i class="pi pi-file-edit p-1 my-2"
                                        style="font-size: 1rem"></i>Custom</div>
                                <div>
                                    <div class="card flex flex-wrap justify-center items-end">
                                        <FloatLabel class="p-5 px-2 w-[200px]">
                                            <DatePicker v-model="startDate" inputId="over_label" showIcon
                                                iconDisplay="input" @focus="onDatePickerFocus"
                                                @blur="onDatePickerBlur" />


                                            <label for="over_label" class="px-2 py-4">Start Date</label>
                                        </FloatLabel>
                                        <FloatLabel class="p-5 px-2 w-[200px]">
                                            <DatePicker v-model="endDate" inputId="over_label" showIcon
                                                iconDisplay="input" @focus="onDatePickerFocus"
                                                @blur="onDatePickerBlur" />


                                            <label for="over_label" class="px-2 py-4">End Date</label>
                                        </FloatLabel>
                                    </div>
                                    <div class="flex justify-end text-end p-4 ">
                                        <div class="text-skyblue" @click="applyCustomDateFilter">Apply</div>
                                    </div>
                                </div>

                            </div>
                        </template>
                    </ConfirmPopup>
                    <InputText type="text" v-model="value" @click="requireConfirmation($event)"
                        placeholder="Selected Date Range" />

                </div>

                <div style="display: flex; align-items: center;">
                    <Button type="button" icon="pi pi-sync" label="" outlined @click="clearFilter"
                        severity="secondary" />
                    <div>
                        <div style="padding-left: 10px; text-align: left;">
                            <MultiSelect v-model="selectedColumns" :options="columns" optionLabel="header" filter
                                :maxSelectedLabels="1" class="w-full sm:w-64"
                                :selectedItemTemplate="selectedItemTemplate">
                                <template #value>
                                    Show/Hide column
                                </template>
                                <template #footer>
                                    <div class="p-3 flex justify-between"
                                        style="padding:3px;display:flex;justify-content:space-between">
                                        <Button label="Add New" severity="secondary" text size="small"
                                            icon="pi pi-plus" />
                                        <Button label="Remove All" severity="danger" text size="small"
                                            icon="pi pi-times" @click="clearAllColumns" />
                                    </div>
                                </template>
                            </MultiSelect>
                        </div>
                    </div>

                    <div style="padding-left: 10px;">
                        <MultiSelect v-model="selectedCategories" :options="categoryOptions" optionLabel="name" filter
                            display="chip" size="small" placeholder="Select Categories" class="w-full sm:w-64">
                        </MultiSelect>
                    </div>
                    <div class="card flex justify-center" style="padding-left:10px">
                        <Button type="button" icon="pi pi-ellipsis-v" @click="togglemenu" aria-haspopup="true"
                            severity="secondary" aria-controls="overlay_menu" />
                        <Menu ref="menu" id="overlay_menu" :model="menus" :popup="true" />
                    </div>
                </div>
            </div>
            <div class="card" style="margin: 20px;">
                <div style="max-height: 600px; overflow: auto;">
                    <DataTable v-model:selection="selectedProduct" :value="filteredProducts" :dataKey="id"
                        :totalRecords="filteredProducts.length"
                        :rowsPerPageOptions="[10, 20, 30, 50, 100, { label: 'All', value: -1 }]" :sortField="sortField"
                        :sortOrder="sortOrder" tableStyle="min-width: 50rem" size="small"
                        @row-click="navigateToProductDetail" scrollable scrollHeight="550px">

                        <Column selectionMode="multiple" headerStyle="width: 3rem"></Column>
                        <Column v-for="col in selectedColumns" :key="col.field" :field="col.field" :header="col.header"
                            :sortable="true">

                            <template #body="slotProps">
                                <div v-if="col.field === 'action'" style="display: flex;">
                                    <Button icon="pi pi-check" @click="visibleRight = true" severity="secondary"
                                        size="small" rounded style="margin: 2px;margin-left:0px !important;" />
                                    <Button icon="pi pi-bookmark" @click="callnewvalue" severity="secondary"
                                        size="small" rounded style="margin: 2px;padding:2px !important;" />
                                    <Button icon="pi pi-user" @click="navigateToProductDetail" severity="secondary"
                                        size="small" rounded style="margin: 2px;padding:2px !important;" />
                                </div>
                                <div v-else>
                                    {{ col.field === 'price' ? formatCurrency(slotProps.data.price) :
                                        col.field === 'date' ? new Date(slotProps.data.date).toLocaleDateString() :
                                            slotProps.data[col.field] }}
                                </div>
                            </template>
                        </Column>
                    </DataTable>
                </div>
            </div>
        </div>

        <Drawer v-model:visible="visibleRight" header="Drawer" position="right">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit...</p>
        </Drawer>

        <Toast />
    </div>
</template>


<script setup>
import { ref, computed, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import Paginator from 'primevue/paginator';
import MultiSelect from 'primevue/multiselect';
import Button from 'primevue/button';
import InputText from 'primevue/inputtext';
import IconField from 'primevue/iconfield';
import InputIcon from 'primevue/inputicon';
import ConfirmPopup from 'primevue/confirmpopup';
import Menu from 'primevue/menu';
import Toast from 'primevue/toast';
import { useToast } from 'primevue/usetoast';
import Drawer from 'primevue/drawer';
import { useConfirm } from "primevue/useconfirm";
import DatePicker from 'primevue/datepicker';

import 'primeicons/primeicons.css'
const startDate = ref(null);
const endDate = ref(null);
const dateFilter = ref(null);
const confirm = useConfirm();
const visibleRight = ref(false);
const toast = useToast();
const router = useRouter();
const products = ref([]);
const selectedProduct = ref([]);
const searchQuery = ref('');
const sortField = ref('price');
const sortOrder = ref(1);
const rowsPerPage = ref(10);
const currentPage = ref(0);
const menu = ref();
const selectedColumns = ref([ // New column configuration including the date
    { field: 'code', header: 'Code' },
    { field: 'name', header: 'Name' },
    { field: 'price', header: 'Price' },
    { field: 'category', header: 'Category' },
    { field: 'quantity', header: 'Quantity' },
    { field: 'date', header: 'Date' }, // Added date column
    { field: 'action', header: 'Action' }
]);
const categoryOptions = ref([
    { name: 'Category A', value: 'Category A' },
    { name: 'Category B', value: 'Category B' },
    { name: 'Category C', value: 'Category C' }
]);
const selectedCategories = ref([]);
const filteredProducts = computed(() => {
    let filtered = products.value;

    if (searchQuery.value) {
        filtered = filtered.filter(product => {
            return Object.values(product).some(value =>
                String(value).toLowerCase().includes(searchQuery.value.toLowerCase())
            );
        });
    }

    if (selectedCategories.value.length > 0) {
        const selectedCategoryValues = selectedCategories.value.map(category => category.value);
        filtered = filtered.filter(product => selectedCategoryValues.includes(product.category));
    }

    // Apply date range filter
    if (dateFilter.value) {
        filtered = filtered.filter(product => {
            const productDate = new Date(product.date);
            return productDate >= dateFilter.value.start && productDate <= dateFilter.value.end;
        });
    }

    filtered.sort((a, b) => {
        const modifier = sortOrder.value === 1 ? 1 : -1;
        if (a[sortField.value] < b[sortField.value]) return -1 * modifier;
        if (a[sortField.value] > b[sortField.value]) return 1 * modifier;
        return 0;
    });

    const start = currentPage.value * rowsPerPage.value;
    const end = start + rowsPerPage.value;
    return filtered.slice(start, end);
});

const fetchProducts = async () => {
    debugger
    try {
        const response = await fetch('/products.json');
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        products.value = await response.json();
        console.log('Fetched Products:', products.value); // Log the fetched products
    } catch (error) {
        console.error('Failed to fetch products:', error);
    }
};


const isInteractingWithDatePicker = ref(false);

const onDatePickerFocus = () => {
    isInteractingWithDatePicker.value = true;
};

const onDatePickerBlur = () => {
    isInteractingWithDatePicker.value = false;
};

const requireConfirmation = (event) => {
    if (isInteractingWithDatePicker.value) return;
    confirm.require({
        // target: event.currentTarget,
        group: 'headless',
    });
};
const formatCurrency = (value) => {
    return value.toLocaleString('en-US', { style: 'currency', currency: 'USD' });
};
onMounted(() => {
    fetchProducts();
});
const togglemenu = (event) => {
    menu.value.toggle(event);
};

const selectedItemTemplate = (selected) => {
    return selected.length ? 'Select Column' : 'Select Columns';
}
const menus = ref([
    {
        items: [
            {
                label: 'Refresh',
                icon: 'pi pi-refresh',
                command: () => handleMenuAction('refresh') // Call handleMenuAction for refresh
            },
            {
                label: 'Export',
                icon: 'pi pi-upload',
                command: () => handleMenuAction('export') // Call handleMenuAction for export
            }
        ]
    }
]);
const handleMenuAction = (action) => {
    if (action === 'export') {
        exportCSV();
    } else if (action === 'refresh') {
        clearFilter();
    }
};
const setDateFilter = (filter) => {
    const now = new Date();
    if (filter === 'week') {
        dateFilter.value = { start: new Date(now.setDate(now.getDate() - 7)), end: new Date() };
    } else if (filter === '15days') {
        dateFilter.value = { start: new Date(now.setDate(now.getDate() - 15)), end: new Date() };
    } else if (filter === 'month') {
        dateFilter.value = { start: new Date(now.setMonth(now.getMonth() - 1)), end: new Date() };
    } else if (filter === '3months') {
        dateFilter.value = { start: new Date(now.setMonth(now.getMonth() - 3)), end: new Date() };
    } else if (filter === 'year') {
        dateFilter.value = { start: new Date(now.setFullYear(now.getFullYear() - 1)), end: new Date() };
    } else if (filter === '6months') {
        dateFilter.value = { start: new Date(now.setMonth(now.getMonth() - 6)), end: new Date() };
    } else if (filter === 'ytd') {
        dateFilter.value = { start: new Date(now.getFullYear(), 0, 1), end: new Date() };
    }
};

// Apply custom date range filter
const applyCustomDateFilter = () => {
    if (startDate.value && endDate.value) {
        // Format the dates if needed
        const startDateStr = startDate.value.toLocaleDateString('en-US');
        const endDateStr = endDate.value.toLocaleDateString('en-US');

        // Set the value field to the formatted date range
        value.value = `${startDateStr} to ${endDateStr}`;

        // Set the date filter for the table
        dateFilter.value = { start: startDate.value, end: endDate.value };
    }
};

</script>

<style scoped>
.card {
    position: relative;
}

.data-table-container {
    max-height: 400px;
    /* Set the height you want */
    overflow-y: auto;
    /* Enable vertical scrolling */
}

.p-datatable {
    width: 100%;
    /* Ensure the table takes full width */
}

.p-datatable-header {
    position: sticky !important;
    /* Make the header sticky */
    top: 0 !important;
    /* Stick to the top */
    z-index: 1 !important;
    /* Ensure it stays above the body */
    background: white;
    /* Set background to avoid transparency issues */
}
</style>