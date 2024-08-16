<script setup lang="ts">
import FCForm from './components/FCForm.vue';
import FCHeader from './components/FCHeader.vue';
import { ref, provide, onMounted } from 'vue';
import InfoModal from './components/InfoModal.vue';
import WarningModal from './components/WarningModal.vue';
import SuccessModal from './components/SuccessModal.vue';

const isStrictMode = ref(false);
const introInfo = ref(true);
const extraInfoShown = ref(false);
const showInfoModal = ref(false);
const wrongIndices = ref<Array<number>>([]);
const showWarningModal = ref(false);
const firstNameValue = ref("");
const showSuccessModal = ref(false);

const fcForm = ref();

provide("sharedIsStrictMode", isStrictMode);

function autoComplete() {
  fcForm?.value.autoCompleteForm();
}

function showWarningModalWithValues(values: Array<number>) {
  wrongIndices.value = values;
  showWarningModal.value = true;
}

function showSuccessModalWithValues(firstName: string) {
  firstNameValue.value = firstName;
  showSuccessModal.value = true;
}

function closeSuccessModal() {
  showSuccessModal.value = false;
  fcForm?.value.reset();
  if (!extraInfoShown.value) {
    introInfo.value = false;
    showInfoModal.value = true;
  }
}

function closeInfoModal() {
  if (!extraInfoShown.value && !introInfo.value) {
    extraInfoShown.value = true;
  }
  showInfoModal.value = false;
  introInfo.value = true;
}

onMounted(() => {
  showInfoModal.value = true;
});
</script>

<template>
  <InfoModal 
    :show="showInfoModal"
    :isIntro="introInfo"
    @closeInfo="closeInfoModal" />
  <WarningModal 
    :show="showWarningModal"
    :wrongIndices="wrongIndices"
    @closeWarning="showWarningModal = false" />
  <SuccessModal 
    :show="showSuccessModal"
    :firstName="firstNameValue"
    @closeSuccess="closeSuccessModal" />
  <div class="md:container md:mx-auto w-full h-full min-h-screen bg-gray-50 p-3 md:py-6 font-aspekta">
    <header class="w-full border border-1 border-border-light rounded-2xl bg-white p-4 mb-3 lg:p-6">
      <FCHeader 
        @openInfo="showInfoModal = true"
        @autoComplete="autoComplete"
        :isStrictMode="isStrictMode"
      />
    </header>

    <main class="w-full border border-1 border-border-light rounded-2xl bg-white p-4 md:mb-2 lg:p-6 flex flex-col items-center gap-4">
      <FCForm ref="fcForm"
        @showWarning="showWarningModalWithValues" 
        @showSuccess="showSuccessModalWithValues"/>
    </main>
  </div>
</template>

<style scoped>

</style>
