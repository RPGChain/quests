<template>
  <div class="space-y-4">
    
    <fwb-input v-model="quest.name" placeholder="Quest Name" label="Quest Name" />
    <fwb-textarea v-model="quest.description" placeholder="Quest Description" label="Quest Description" />
    <fwb-input v-model="quest.image" placeholder="Image URL" label="Image URL" />
    <fwb-input v-model="quest.playable_starts_at" placeholder="Playable Starts At (YYYY-MM-DDTHH:mm:ss)" label="Playable Starts At" />
    <fwb-input v-model="quest.playable_ends_at" placeholder="Playable Ends At (YYYY-MM-DDTHH:mm:ss)" label="Playable Ends At" />
    <fwb-switch v-model="quest.private" label="Private" />

    <!-- Rolls -->
    <div v-for="(roll, index) in quest.rolls" :key="index" class="border p-4">
      <fwb-input v-model="roll.dc" label="Difficulty Class (DC)" placeholder="Enter DC" />
      <fwb-input v-model="roll.key" label="Roll Key" placeholder="Enter Key" />
      <fwb-input v-model="roll.label" label="Label" placeholder="Enter Label" />
      <fwb-input v-model="roll.actionSuccess" label="Action on Success" placeholder="Enter Action Success" />
      <fwb-input v-model="roll.actionFail" label="Action on Failure" placeholder="Enter Action Fail" />
      <fwb-button @click="removeRoll(index)">Remove Roll</fwb-button>
    </div>
    <fwb-button @click="addRoll">Add Roll</fwb-button>

    <!-- Pages -->
    <div v-for="(page, index) in quest.pages" :key="index" class="border p-4">
      <fwb-input v-model="page.key" label="Page Key" placeholder="Enter Page Key" />
      <fwb-textarea v-model="page.markdown" label="Markdown Content" placeholder="Enter Markdown" />
      <fwb-input v-model="page.image" label="Image URL" placeholder="Enter Image URL" />
      <fwb-input v-model="page.xp" label="Experience Points (XP)" placeholder="Enter XP" type="number" />
      <!-- Options and DisplayRolls are more complex and might require dedicated subcomponents -->
      <fwb-button @click="removePage(index)">Remove Page</fwb-button>
    </div>
    <fwb-button @click="addPage">Add Page</fwb-button>

    <!-- NFT Contract Addresses -->
    <div v-for="(address, index) in quest.token_contracts" :key="index" class="border p-4">
      <fwb-input 
        :value="address" 
        @input="event => updateContractAddress(event, index)" 
        label="NFT Contract Address" 
        placeholder="Enter Address" 
      />
      <fwb-button @click="removeContractAddress(index)">Remove Address</fwb-button>
    </div>
    <fwb-button @click="addContractAddress">Add NFT Contract Address</fwb-button>

  </div>
</template>

<script lang="ts" setup>
import { defineProps, ref } from 'vue'
import { FwbInput, FwbButton } from 'flowbite-vue'

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
