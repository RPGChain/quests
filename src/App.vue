<script setup>
import { ref, reactive, watch } from 'vue';
import { 
  FwbNavbar, 
  FwbNavbarLogo,
  FwbButton,
  FwbHeading,
  FwbP,
  FwbFooter,
  FwbFooterCopyright,
  FwbFooterLink,
  FwbFooterLinkGroup,
 } from 'flowbite-vue';
import logoTextImage from './assets/img/logo-text.svg';
import QuestForm from './components/QuestForm.vue';

// Initial empty structure of the quest object
const quest = reactive({
  // Define the structure according to your quest schema
  name: '',
  description: '',
  image: '',
  // ... other properties
});

// Watcher to monitor changes to the quest object
watch(quest, (newQuest) => {
  // Update logic (if any) when quest changes
});

// Method to convert the quest object to a JSON string for display
const questJson = () => JSON.stringify(quest, null, 2);
</script>

<template>
  <FwbNavbar class="border-gray-200 bg-white px-2 sm:px-4 py-2.5 dark:bg-gray-900 shadow">
    <FwbNavbarLogo alt="RPGChain Quest Creator" :image-url="logoTextImage" link="#" />
    <template #right-side>
      <fwb-button gradient="green" class="mr-4">
        Play Quest
      </fwb-button>
      <fwb-button gradient="purple">
        Download
      </fwb-button>
    </template>
  </FwbNavbar>

  <main class="container flex flex-wrap justify-between items-center mx-auto mb-8 pb-8">
    <section>

      <div class="mt-4">
        <fwb-heading tag="h1" class="mb-4">Quest Builder</fwb-heading>
        <fwb-p class="mb-2">
          This tool provides an easy way to generate JSON files in the RPGChain Quest Format.
        </fwb-p>
        <fwb-p class="mb-2">
          Use the form below to create your quest. Once you are done, you can use the buttons above to preview and test out your quest, and download the quest file. When you're complete, <a href="#" class="underline">click here to learn about submitting your quest to RPGChain.</a>
        </fwb-p>
      </div>
    </section>

    <div class="flex flex-col md:flex-row w-full">
      <section class="md:w-1/2">
        <QuestForm :quest="quest" />
      </section>
      <section class="ml-8 md:w-1/2 pl-4 bg-gray-800 text-gray-100 p-4">
        <pre>{{ questJson() }}</pre>
      </section>
    </div>

  </main>

    <fwb-footer>
      <fwb-footer-copyright
        by="RPGChain"
        href="https://rpgchain.com"
        copyright-message=""
      />
      <fwb-footer-link-group>
        <fwb-footer-link href="https://rpgchain.com">
          Visit RPGChain
        </fwb-footer-link>
      </fwb-footer-link-group>
    </fwb-footer>

</template>
