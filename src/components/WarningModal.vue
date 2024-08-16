<script setup lang="ts">
import Labels from './data/Labels';

defineProps({
  show: { type: Boolean, default: false },
  wrongIndices: { type: Array<number>, default: [] }
});
</script>

<template>
  <div class="absolute z-10 top-0 left-0 w-dvw h-dvh backdrop-blur-sm bg-black/30"
    v-if="show"
    @click="$emit('closeWarning')">
    <div class="w-full h-full flex justify-center items-center">
      <div class="md:w-1/4 w-full border border-1 border-border-light rounded-2xl bg-white p-0 overflow-hidden"
        @click.stop="">
        <div class="flex justify-between items-center pl-4 border-b border-b-1 border-border-light bg-red-100">
            <p class="text-2xl text-red-500">OOPS</p>
          <button class="h-12 w-12"
            @click="$emit('closeWarning')">
            <Icon :icon="['fas','xmark']" />
          </button>
        </div>
        <div class="p-4 flex flex-col items-end">
          <article>
            <p>You made some mistakes filling in the CAPTCHAForm.
              I can not determine if you're a robot, or a human. 
              Please correct the following fields:</p>
            <p 
            v-for="ind in wrongIndices"
            :key="ind">
              - {{ Labels[ind].value }}
            </p>
            <div class="flex justify-end">
              <button class="mt-4 p-2 border border-1 border-border-light rounded-md text-white bg-red-500"
                @click="$emit('closeWarning')">
                <span class="font-medium">Try again</span>
              </button>
            </div>
          </article>
        </div>
      </div>
    </div>
  </div>
</template>