<script setup lang="ts">
import { inject, reactive, ref, type Ref } from 'vue';
import draggable from 'vuedraggable'
import Fields from './data/Fields';
import Labels from './data/Labels';

defineExpose({
  autoCompleteForm,
  reset
});

const emit = defineEmits(['showSuccess', 'showWarning']);

const wrongIndices: Array<number> = [];

const formFields = reactive(Fields);
const formFieldsLabels = Labels;
const dragging = ref(false);

const isStrictMode = inject<Ref<boolean>>("sharedIsStrictMode");

const isSolvingCaptcha = ref(false);

function autoCompleteForm() {
  sortFormFields();

  formFields[0].value = "Lewis";
  formFields[1].value = "Menelaws";
  formFields[2].value = "1998-11-26";
  formFields[3].value = "me@lewismenelaws.com";
  formFields[4].value = "me@lewismenelaws.com";
  formFields[5].value = "$ecreT1234!";
  formFields[6].value = "$ecreT1234!";
  formFields[7].value = "Bloor St W";
  formFields[8].value = "618";
  formFields[9].value = "Toronto";
}

function reset() {
  sortFormFields();
  isSolvingCaptcha.value = false;

  formFields[0].value = "";
  formFields[1].value = "";
  formFields[2].value = "";
  formFields[3].value = "";
  formFields[4].value = "";
  formFields[5].value = "";
  formFields[6].value = "";
  formFields[7].value = "";
  formFields[8].value = "";
  formFields[9].value = "";
}

function submit() {
  if (!isSolvingCaptcha.value) { 
    if (isStrictMode!.value) {
      shuffleFormFieldsStrictMode();
    } else {
      shuffleFormFieldsNotStrictMode();
    }
    isSolvingCaptcha.value = true;
  } else {
    checkAnswers();
  }
}

function checkAnswers() {
  if (isStrictMode!.value) {
    if(isStrictModeCorrect()) {
      emit("showSuccess", formFields[0].value);
      isSolvingCaptcha.value = false;
    } else {
      emit("showWarning", wrongIndices);
    }
  } else {
    emit("showSuccess", formFields[0].value);
    isSolvingCaptcha.value = false;
  }
}

function isStrictModeCorrect() {
  wrongIndices.splice(0, wrongIndices.length);
  let result = true;

  for(let i = 0; i < formFields.length; i++) {
    if (formFields[i].id !== formFieldsLabels[i].id) {
      wrongIndices.push(i);
      result = false;
    }
  }

  return result;
}

function sortFormFields() {
  formFields.sort((a, b) => a.order - b.order);
}

function shuffleFormFieldsNotStrictMode() {
  for (let i = formFields.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [formFields[i], formFields[j]] = [formFields[j], formFields[i]];
  }
}

function shuffleFormFieldsStrictMode() {
    [formFields[3], formFields[4]] = [formFields[4], formFields[3]];

    [formFields[5], formFields[6]] = [formFields[6], formFields[5]];

    const otherIndices = [0, 1, 2, 7, 8, 9];

    for (let i = otherIndices.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        const idx1 = otherIndices[i];
        const idx2 = otherIndices[j];
        [formFields[idx1], formFields[idx2]] = [formFields[idx2], formFields[idx1]];
    }
}

</script>

<template>
  <p class="invisible md:visible text-3xl">Please register yourself!</p>
  <div class="grid grid-cols-4 grid-rows-10 gap-4">
    <div class="grid grid-rows-subgrid row-span-10 col-span-1 md:col-span-2">
      <p class="flex items-center"
      v-for="label in formFieldsLabels"
        :key="label.id">
        {{ label.value }}
      </p>
    </div>

    <draggable class="grid grid-rows-subgrid row-span-10 col-span-3 md:col-span-2"
      :list="formFields"
      :animation="200"
      :disabled="!isSolvingCaptcha"
      ghost-class="ghost"
      item-key="id"
      @start="dragging = true"
      @end="dragging = false">
      <template #item="{ element }">
        <div class="flex gap-2 items-center justify-end">
          <Icon :icon="['fas', 'grip-vertical']" 
            class="cursor-pointer"
            v-if="isSolvingCaptcha"/>
          <input class="w-52 rounded-md py-2 px-3 bg-gray-50 border border-1 border-border-light"
            v-model="element.value" 
            :type="element.type" />
        </div>
      </template>
    </draggable>
  </div>
  <button class="w-max p-2 rounded-md text-white bg-green-500"
    @click="submit">
    <span class="font-medium">Submit</span>
  </button>
</template>