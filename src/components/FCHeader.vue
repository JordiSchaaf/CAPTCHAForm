<script setup lang="ts">
import { ref } from 'vue';
import SettingsMenu from './SettingsMenu.vue';

const emit = defineEmits(['autoComplete', 'openInfo']);
const isSettingsOpen = ref(false);

function autoComplete() {
  isSettingsOpen.value = false;
  emit("autoComplete");
}
</script>

<template>
  <div class="flex justify-between">
    <div class="flex items-center">
      <img src="../assets/img/recaptcha-project-browser-transparent-2.png" 
        class="h-12"/>
      <p class="invisible md:visible text-3xl">CAPTCHAForm</p>
    </div>
    <div class="flex gap-4">
      <div class="relative" id="settingsMenu">
        <button class="border border-1 border-border-light p-2 rounded-full h-12 w-12"
          @click="isSettingsOpen = !isSettingsOpen">
          <Icon :icon="['fas','gears']" />
        </button>
        <SettingsMenu 
          :show="isSettingsOpen" 
          @closeSettings="isSettingsOpen = false"
          @autoComplete="autoComplete"/>
      </div>
      <button class="border border-1 border border-border-light p-2 rounded-full h-12 w-12"
        @click="$emit('openInfo')">
        <Icon :icon="['fas','info']" />
      </button>
    </div>
  </div>
</template>