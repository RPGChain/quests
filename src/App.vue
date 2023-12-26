<script setup>
import { ref, reactive } from 'vue';
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
  FwbModal
 } from 'flowbite-vue';
import logoTextImage from './assets/img/logo-text.svg';
import QuestForm from './components/QuestForm.vue';
import QuestPlayer from '@written-rpg/quest-player';

const quest = reactive({
  name: '',
  description: '',
  image: '',
});

const isShowingQuestPlayerModal = ref(false);

const questJson = () => JSON.stringify(quest, null, 2);

const closeQuestPlayerModal = () => {
  isShowingQuestPlayerModal.value = false;
};
const showQuestPlayerModal = () => {
  isShowingQuestPlayerModal.value = true;
};
</script>

<template>
  <FwbNavbar class="border-gray-200 bg-white px-2 sm:px-4 py-2.5 dark:bg-gray-900 shadow">
    <FwbNavbarLogo alt="RPGChain Quest Creator" :image-url="logoTextImage" link="#" />
    <template #right-side>
      <fwb-button gradient="green" class="mr-4" @click="showQuestPlayerModal">
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

    <fwb-modal v-if="isShowingQuestPlayerModal" @close="closeQuestPlayerModal">
      <template #header>
        <h5 class="text-lg font-medium leading-normal text-gray-800">
          Play Quest
        </h5>
      </template>

      <template #body>
        <QuestPlayer :quest="quest" />
      </template>

      <template #footer>
        <fwb-button @click="closeQuestPlayerModal">
          Close
        </fwb-button>
      </template>
    </fwb-modal>

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
