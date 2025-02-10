<template>
  <div class="grid grid-cols-12 ">
    <div class="col-span-8 border-x border-t border-gray">
      <Tabs value="0">
        <TabList>
          <Tab v-for="tab in tabs" :key="tab.title" :value="tab.value">
            <i :class="tab.icon" style="margin-right: 8px;"></i>
            {{ tab.title }}
          </Tab>
        </TabList>
        <TabPanels>
          <TabPanel v-for="tab in tabs" :key="tab.content" :value="tab.value">
            <p class="m-0">{{ tab.content }}</p>
          </TabPanel>
        </TabPanels>
      </Tabs>
    </div>
    <div class="col-span-4  ">
      <div class="border border-gray p-3 py-4 text-lg font-semibold "> CRM-LEAD-2025-0012 </div>
      <div class="border border-gray p-3">
        <div class="flex ">

          <Avatar label="P" class="mr-2" size="large" shape="circle" />
          <div>
            <div class="text-lg pb-1 font-semibold">Darlena Robertson</div>
            <div class="flex gap-2">
              <div class="p-1 px-2 rounded-md bg-gray-100 gap-2">
                <i class="pi pi-envelope" style="font-size: 15px"></i>
              </div>
              <div class="p-1 px-2 rounded-md bg-gray-100 ">
                <i class="pi pi-paperclip" style="font-size: 15px"></i>
              </div>
              <div class="p-1 px-2 rounded-md bg-gray-100 ">
                <i class="pi pi-link" style="font-size: 15px"></i>
              </div>
            </div>
          </div>

        </div>

      </div>
      <div class="border-x border-gray">





        <div class="flex">
          <div>
            <div class="px-5 py-2">Teritary</div>
            <div class="px-5 py-2">Website</div>
            <div class="px-5 py-2">organization</div>
            <div class="px-5 py-2">Industry</div>
            <div class="px-5 py-2">Job Title</div>
            <div class="px-5 py-2">Source</div>
            <div class="px-5 py-2">Lead Owner</div>
          </div>
          <div>
            <div  class="py-2">Add territary..</div>
            <div  class="py-2">https://test.com</div>
            <div  class="py-2">Sony</div>
            <div  class="py-2">Others</div>
            <div  class="py-2">COO</div>
            <div  class="py-2">Web</div>
            <div  class="py-2">Sharik</div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue';
import Tabs from 'primevue/tabs';
import TabList from 'primevue/tablist';
import Tab from 'primevue/tab';
import TabPanels from 'primevue/tabpanels';
import TabPanel from 'primevue/tabpanel';

import Avatar from 'primevue/avatar';
import AvatarGroup from 'primevue/avatargroup';   //Optional for grouping

const tabs = ref([
  { title: 'Activity', content: 'Tab 1 Content', value: '0', icon: 'pi pi-star' },
  { title: 'Email', content: 'Tab 2 Content', value: '1', icon: 'pi pi-envelope' },
  { title: 'Comment', content: 'Tab 3 Content', value: '2', icon: 'pi pi-comment' },
  { title: 'Data', content: 'Tab 4 Content', value: '3', icon: 'pi pi-server' },
  { title: 'Task', content: 'Tab 5 Content', value: '4', icon: 'pi pi-check-circle' },
  { title: 'Note', content: 'Tab 6 Content', value: '5', icon: 'pi pi-file' },
  { title: 'Attachment', content: 'Tab 7 Content', value: '6', icon: 'pi pi-paperclip' },

]);
</script>

<style >
.p-drawer-content{
    flex-grow: none !important;
    overflow-y:none !important;
    padding-left: none !important;
    padding-right:none !important;
    padding-top: none !important;
    padding-bottom: none !important;
}
.p-drawer-header {
    display: none !important;
    flex-shrink: 0;
    align-items: none ;
    justify-content: space-between;
    padding: 0px !important;
}
</style>