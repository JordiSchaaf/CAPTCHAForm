<script setup lang="ts">
defineProps({
  show: { type: Boolean, default: false },
  firstName: { type: String }
});
</script>

<template>
  <div class="absolute z-10 top-0 left-0 w-dvw h-dvh backdrop-blur-sm bg-black/30"
    v-if="show"
    @click="$emit('closeSuccess')">
    <div class="w-full h-full flex justify-center items-center">
      <div class="md:w-1/4 w-full border border-1 border-border-light rounded-2xl bg-white p-0 overflow-hidden"
        @click.stop="">
        <div class="flex justify-between items-center pl-4 border-b border-b-1 border-border-light bg-green-100">
            <p class="text-2xl">Success</p>
          <button class="h-12 w-12"
            @click="$emit('closeSuccess')">
            <Icon :icon="['fas','xmark']" />
          </button>
        </div>
        <div class="p-4 flex flex-col items-end">
          <article>
            <p class="text-xl">Welcome {{ firstName }}!</p>
            <p>It is great to have some human interaction!</p> 
          </article>
          <button class="p-2 border border-1 border-border-light rounded-md text-white bg-green-500"
            @click="$emit('closeSuccess')">
            <span class="mr-2 font-medium">I agree</span>
            <Icon :icon="['fas', 'check']" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>