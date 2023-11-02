<script setup lang="ts">
import { ref } from "vue";
const currentCalculationValue = ref("");

function handleClick(value: string) {
  const operationValues = ["/", "*", "-", "+"];
  switch (value) {
    case "clear":
      currentCalculationValue.value = "";
      break;

    case "equals":
      if(currentCalculationValue.value.length > 1) {
        const lastChar = currentCalculationValue.value.slice(-1);
        if (operationValues.includes(lastChar)) {
          break;
        }
      }
      currentCalculationValue.value = eval(currentCalculationValue.value);
      break;

    case "remove":
      currentCalculationValue.value = currentCalculationValue.value.slice(
        0,
        -1
      );
      break;

    case ".":
      if(currentCalculationValue.value.length > 0 && currentCalculationValue.value.includes(".")) {
        break;
      }
      currentCalculationValue.value += value;
      break;

    default:
      const lastChar = currentCalculationValue.value.toString().slice(-1);
      if (
        operationValues.includes(value) &&
        operationValues.includes(lastChar)
      ) {
        currentCalculationValue.value = currentCalculationValue.value.slice(
          0,
          -1
        );
      }

      if(operationValues.includes(value) && currentCalculationValue.value.length > 0) {
        currentCalculationValue.value = eval(currentCalculationValue.value);
      }
      currentCalculationValue.value += value;
      break;
  }
}

const buttons = ref([
  { value: "clear", label: "C" },
  { value: "remove", label: "&LeftArrow;", classes: "col-span-2" },
  { value: "/", label: "&divide;" },
  { value: "7", label: "7" },
  { value: "8", label: "8" },
  { value: "9", label: "9" },
  { value: "*", label: "&cross;" },
  { value: "4", label: "4" },
  { value: "5", label: "5" },
  { value: "6", label: "6" },
  { value: "-", label: "&minus;" },
  { value: "1", label: "1" },
  { value: "2", label: "2" },
  { value: "3", label: "3" },
  { value: "+", label: "&plus;" },
  { value: "0", label: "0", classes: "col-span-2" },
  { value: ".", label: "." },
  { value: "equals", label: "&equals;" },
]);
</script>
<template>
  <div class="w-96 bg-slate-500 p-4 m-auto">
      <div
        class="w-full h-32 bg-slate-800 border border-gray-900 mb-6 text-right px-4 pt-14"
      >
        <span class="text-6xl text-white">{{
          currentCalculationValue || 0
        }}</span>
      </div>
      <div class="">
        <div class="grid grid-cols-4 gap-2.5">
          <span
            v-for="({ value, label, classes }, index) in buttons"
            :key="index"
            :class="classes"
          >
            <button
              v-html="label"
              @click.prevent="handleClick(value)"
              class="bg-gray-800 text-center w-20 h-20 text-white font-bold py-5 text-3xl"
            ></button>
          </span>
        </div>
      </div>
    </div>
</template>
