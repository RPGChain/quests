<template>
  <div class="space-y-4 p-4 rounded bg-white shadow">
    
    <fwb-input class="mb-2" v-model="quest.name" placeholder="Quest Name" label="Quest Name" />

    <fwb-textarea class="mb-0" v-model="quest.description" placeholder="Quest Description" label="Quest Description">
      <template #helper>
        A short hook describing your quest.
      </template>
    </fwb-textarea>

    <fwb-input class="mb-0" v-model="quest.image" placeholder="Image URL" label="Image URL">
      <template #helper>
        An external url to an image to display as a teaser for this quest. 1280x720 is recommended.
      </template>
    </fwb-input>

    <fwb-accordion class="mb-4" :open-first-item="false">
    <fwb-accordion-panel>
      <fwb-accordion-header>Permissions</fwb-accordion-header>
      <fwb-accordion-content>
        <fwb-input v-model="quest.playable_starts_at" placeholder="Playable Starts At (YYYY-MM-DDTHH:mm:ss)" label="Playable Starts At">
          <template #helper>
            If the quest is only playable for a short amount of time, add the date it will become available
          </template>
        </fwb-input>
        <fwb-input class="mt-4" v-model="quest.playable_ends_at" placeholder="Playable Ends At (YYYY-MM-DDTHH:mm:ss)" label="Playable Ends At">
          <template #helper>
            If the quest is only playable for a short amount of time, add the date it will no longer be available to play
          </template>
        </fwb-input>
        <fwb-checkbox class="mt-4" v-model="quest.private" label="Private">
          <template #helper>
            Check this if the quest should not be available in public listings
          </template>
        </fwb-checkbox>
        <label class="mt-4 block mb-2 text-sm font-medium text-gray-900 dark:text-white">Limit access by NFT</label>
        <fwb-p class="opacity-75 text-xs">Be default, any user can play your quest. If you wish to only let your quest be playable by owners of a specific NFT. Enter the contract address to the NFT here.</fwb-p>

        <fwb-table v-if="quest.token_contracts && quest.token_contracts.length > 0">
          <fwb-table-head>
            <fwb-table-head-cell>Contract Address</fwb-table-head-cell>
            <fwb-table-head-cell>
              <span class="sr-only">Edit</span>
            </fwb-table-head-cell>
          </fwb-table-head>
          <fwb-table-body>
            <fwb-table-row v-for="(address, index) in quest.token_contracts" :key="index">
              <fwb-table-cell>
                <fwb-input 
                  :value="address" 
                  @input="event => updateContractAddress(event, index)" 
                  label="NFT Contract Address" 
                  placeholder="Enter Address" 
                />
              </fwb-table-cell>
              <fwb-table-cell>
                <fwb-button @click="removeContractAddress(index)">Remove Address</fwb-button>
              </fwb-table-cell>
            </fwb-table-row>
          </fwb-table-body>
        </fwb-table>

        <fwb-button @click="addContractAddress" class="mt-4">Add NFT Contract Address</fwb-button>

      </fwb-accordion-content>
    </fwb-accordion-panel>
    </fwb-accordion>

    <fwb-accordion :open-first-item="false" class="border border-gray-300 rounded-xl">
    <fwb-accordion-panel>
      <fwb-accordion-header>Rolls</fwb-accordion-header>
      <fwb-accordion-content>

        <div v-if="quest.rolls && quest.rolls.length > 0">
            <div v-for="(roll, index) in quest.rolls" :key="index" class="flex border-l my-2 py-2">
              <div>
                <div class="flex">
                  <fwb-input v-model="roll.key" label="Name" placeholder="Name" />
                  <fwb-input v-model="roll.label" label="Label" placeholder="Enter Label" />
                  <fwb-input v-model="roll.dc" label="Difficulty Class (DC)" placeholder="Enter DC" />
                </div>
                
                <div class="flex">
                  <fwb-input v-model="roll.actionSuccess" label="Action on Success" placeholder="Enter Action Success" />
                  <fwb-input v-model="roll.actionFail" label="Action on Failure" placeholder="Enter Action Fail" />
                </div>
                
              </div>
              <div class="pl-4">
                <fwb-button @click="removeRoll(index)">Remove Roll</fwb-button>
              </div>
            </div>
        </div>

        <fwb-button @click="addRoll">Add Roll</fwb-button>
      </fwb-accordion-content>
    </fwb-accordion-panel>
    </fwb-accordion>

    <fwb-heading tag="h2">Pages</fwb-heading>
    <!-- Pages -->
    <div v-for="(page, index) in quest.pages" :key="index" class="border p-4">
      <fwb-input v-model="page.key" label="Page Name" placeholder="Enter Page Key" />
      <fwb-textarea v-model="page.markdown" label="Markdown Content" placeholder="Enter Markdown" />
      <fwb-input v-model="page.image" label="Image URL" placeholder="Enter Image URL" />
      <fwb-input v-model="page.xp" label="Experience Points (XP)" placeholder="Enter XP" type="number" />
      <!-- Options and DisplayRolls are more complex and might require dedicated subcomponents -->
      <fwb-button @click="removePage(index)">Remove Page</fwb-button>
    </div>
    <fwb-button @click="addPage">Add Page</fwb-button>

  </div>
</template>

<script lang="ts" setup>
import { defineProps, ref } from 'vue'
import { 
  FwbInput, 
  FwbTextarea, 
  FwbButton, 
  FwbHeading, 
  FwbP,
  FwbCheckbox,
  FwbAccordion,
  FwbAccordionContent,
  FwbAccordionHeader,
  FwbAccordionPanel, 
  FwbTable,
  FwbTableBody,
  FwbTableCell,
  FwbTableHead,
  FwbTableHeadCell,
  FwbTableRow,
} from 'flowbite-vue'

const props = defineProps({
  quest: Object
});

const initializeArrayProperty = (propertyName) => {
  if (!Array.isArray(props.quest[propertyName])) {
    props.quest[propertyName] = [];
  }
};

const addRoll = () => {
  initializeArrayProperty('rolls');
  props.quest.rolls.push({
    dc: '',
    key: '',
    label: '',
    actionSuccess: '',
    actionFail: ''
  });
};

const removeRoll = (index) => {
  props.quest.rolls.splice(index, 1);
};

const addPage = () => {
  initializeArrayProperty('pages');
  props.quest.pages.push({
    key: '',
    markdown: '',
    image: '',
    options: [],
    displayRolls: [],
    xp: 0,
    completionStatus: ''
    // Initialize other properties as needed
  });
};

const removePage = (index) => {
  props.quest.pages.splice(index, 1);
};

const addContractAddress = () => {
  initializeArrayProperty('token_contracts');
  props.quest.token_contracts.push('');
};

const removeContractAddress = (index) => {
  props.quest.token_contracts.splice(index, 1);
};

const updateContractAddress = (event, index) => {
  props.quest.token_contracts[index] = event.target.value;
};

</script>
