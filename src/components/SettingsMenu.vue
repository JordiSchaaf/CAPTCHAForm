<script setup lang="ts">
import { inject, onMounted, ref } from 'vue';
defineProps({
  show: { type: Boolean, default: false },
});
const emit = defineEmits(['autoComplete', 'closeSettings']);

const settingsMenuRef = ref();
const isStrictMode = inject("sharedIsStrictMode");

onMounted(() => {
  document.addEventListener("click", (e) => {
    const settingsMenu = document.getElementById("settingsMenu");
    if (settingsMenu && !settingsMenu.contains(e.target as any)) {
      emit("closeSettings");
    }
  });
});
</script>

<template>
  <div v-if="show"
    class="absolute right-0 border border-1 border-border-light rounded-2xl bg-white p-4 w-max"
    ref="settingsMenuRef">
    <div class="flex flex-col gap-4">
      <div class="flex items-center gap-4">
        <span>Strict mode</span>
        <label class="relative inline-flex items-center justify-center cursor-pointer">
          <input type="checkbox" v-model="isStrictMode" class="sr-only peer">
          <div class="w-9 h-5 bg-gray-200 hover:bg-gray-300 peer-focus:outline-0 rounded-full peer transition-all ease-in-out duration-500 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all  peer-checked:bg-green-500 hover:peer-checked:bg-green-600 "></div>
        </label>
      </div>
      <button class="p-2 border border-1 border-border-light rounded-md text-white bg-blue-400"
        @click="$emit('autoComplete')">
        <span class="font-medium">Fill Form</span>
      </button>
    </div>
  </div>
</template>