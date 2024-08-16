<script setup lang="ts">
defineProps({
  show: { type: Boolean, default: false },
  isIntro: { type: Boolean, default: true }
});
</script>

<template>
  <div class="absolute z-10 top-0 left-0 w-dvw h-dvh backdrop-blur-sm bg-black/30"
    v-if="show"
    @click="$emit('closeInfo')">
    <div class="w-full h-full flex justify-center items-center">
      <div class="md:w-1/3 w-full border border-1 border-border-light rounded-2xl bg-white p-0 overflow-hidden"
        @click.stop="">
        <div class="flex justify-between items-center pl-4 border-b border-b-1 border-border-light">
            <p class="text-2xl">Information</p>
          <button class="h-12 w-12"
            @click="$emit('closeInfo')">
            <Icon :icon="['fas','xmark']" />
          </button>
        </div>
        <div class="p-4 flex flex-col items-end">
          <article v-if="isIntro">
            Welcome to CAPTCHAForm. This is my submission for the 
            <a href="https://hackathon.lewismenelaws.com/" target="_blank">Hackathon with Lewis</a>. 
            CAPTCHAForm is a combination of a captcha and a form, which takes security to the next level.
            Besides that it comes with some other benefits: you can check your answers again and it 
            provides a challenge for people who think normal captchas are too easy. 
            So, buckle up to prove you're no robot!
          </article>
          <article v-if="!isIntro">
            Under the Settings-button, you can change how strict the checks are of the CAPTCHAForm.
            Besides that we provided an autofill function to help you try out the form more easily!
          </article>
          <button class="p-2 border border-1 border-border-light rounded-md text-white bg-green-500"
            @click="$emit('closeInfo')">
            <span class="mr-2 font-medium">Get started</span>
            <Icon :icon="['fas', 'arrow-right']" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>