<template>
  <div
    class="relative top-44 w-[70%] mx-auto flex sm:flex-row items-center justify-between flex-col"
  >
    <Combobox v-model="selectedEnergyLeft">
      <div class="relative sm:mt-1 z-50 mt-10">
        <div
          class="relative w-full cursor-default overflow-hidden rounded-lg bg-white text-left shadow-md focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75 focus-visible:ring-offset-2 focus-visible:ring-offset-teal-300 sm:text-sm"
        >
          <ComboboxInput
            class="w-full border-none py-2 pl-3 pr-10 text-sm leading-5 text-gray-900 focus:ring-0"
            @change="queryLeft = $event.target.value"
          />
          <ComboboxButton
            class="absolute inset-y-0 right-0 flex items-center pr-2"
          >
            <ChevronDownIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
          </ComboboxButton>
        </div>
        <TransitionRoot
          leave="transition ease-in duration-100"
          leave-from="opacity-100"
          leave-to="opacity-0"
          @after-leave="queryLeft = ''"
        >
          <ComboboxOptions
            class="absolute mt-1 max-h-60 w-full overflow-auto rounded-md bg-white py-1 text-base shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none sm:text-sm"
          >
            <div
              v-if="filteredEnergyLeft.length === 0 && queryLeft !== ''"
              class="relative cursor-default select-none py-2 px-4 text-gray-700"
            >
              Nothing found.
            </div>
            <ComboboxOption
              v-for="energy in filteredEnergyLeft"
              as="template"
              :key="energy"
              :value="energy"
              v-slot="{ selected, active }"
            >
              <li
                class="relative cursor-default select-none py-2 pl-10 pr-4"
                :class="{
                  'bg-teal-600 text-white': active,
                  'text-gray-900': !active,
                }"
              >
                <span
                  class="block truncate"
                  :class="{
                    'font-medium': selected,
                    'font-normal': !selected,
                  }"
                >
                  {{ energy }}
                </span>
                <span
                  v-if="selected"
                  class="absolute inset-y-0 flex items-center pl-5"
                  :class="{ 'text-white': active, 'text-teal-600': !active }"
                >
                  <CheckIcon class="h-5 w-5" aria-hidden="true" />
                </span>
              </li>
            </ComboboxOption>
          </ComboboxOptions>
        </TransitionRoot>
      </div>
    </Combobox>
    <Combobox v-model="selectedEnergyRight">
      <div class="relative mt-1">
        <div
          class="relative w-full cursor-default overflow-hidden rounded-lg bg-slate-400 text-left shadow-md focus:outline-none focus-visible:ring-opacity-75 focus-visible:ring-offset-2 focus-visible:ring-offset-teal-300 sm:text-sm"
        >
          <ComboboxInput
            class="w-full border-none py-2 pl-3 pr-10 text-sm leading-5 text-gray-900 focus:ring-0"
            @change="queryRight = $event.target.value"
          />
          <ComboboxButton
            class="absolute inset-y-0 right-0 flex items-center pr-2"
          >
            <ChevronDownIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
          </ComboboxButton>
        </div>
        <TransitionRoot
          leave="transition ease-in duration-100"
          leave-from="opacity-100"
          leave-to="opacity-0"
          @after-leave="queryRight = ''"
        >
          <ComboboxOptions
            class="absolute mt-1 max-h-60 w-full overflow-auto rounded-md bg-white py-1 text-base shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none sm:text-sm"
          >
            <div
              v-if="filteredEnergyRight.length === 0 && queryRight !== ''"
              class="relative cursor-default select-none py-2 px-4 text-gray-700"
            >
              Nothing found.
            </div>
            <ComboboxOption
              v-for="energy in filteredEnergyRight"
              as="template"
              :key="energy"
              :value="energy"
              v-slot="{ selected, active }"
            >
              <li
                class="relative cursor-default select-none py-2 pl-10 pr-4"
                :class="{
                  'bg-teal-600 text-white': active,
                  'text-gray-900': !active,
                }"
              >
                <span
                  class="block truncate"
                  :class="{
                    'font-medium': selected,
                    'font-normal': !selected,
                  }"
                >
                  {{ energy }}
                </span>
                <span
                  v-if="selected"
                  class="absolute inset-y-0 flex items-center pl-5"
                  :class="{ 'text-white': active, 'text-teal-600': !active }"
                >
                  <CheckIcon class="h-5 w-5" aria-hidden="true" />
                </span>
              </li>
            </ComboboxOption>
          </ComboboxOptions>
        </TransitionRoot>
      </div>
    </Combobox>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, type Ref } from "vue";
import {
  Combobox,
  ComboboxInput,
  ComboboxOptions,
  ComboboxOption,
  ComboboxButton,
  TransitionRoot,
} from "@headlessui/vue";
import { CheckIcon, ChevronDownIcon } from "@heroicons/vue/20/solid";

const energies = [
  "AM",
  "BE",
  "RP",
  "CR",
  "EU",
  "FE",
  "gJ",
  "BTU",
  "J",
  "Li",
  "MJ",
  "RF",
  "W",
  "T",
  "Tn",
  "V",
  "8I",
  "AE",
  "IF",
];
const filterEnergy = (query: Ref<string>, selectedEnergy: Ref<string>) => {
  return query.value === ""
    ? energies.filter((item) => item !== selectedEnergy.value)
    : energies.filter(
        (energy) =>
          energy.toLowerCase().includes(query.value.toLowerCase()) &&
          energy !== selectedEnergy.value
      );
};
const selectedEnergyLeft = ref(energies[0]);
const selectedEnergyRight = ref(energies[1]);
const queryLeft = ref("");
const queryRight = ref("");
const filteredEnergyLeft = computed(() =>
  filterEnergy(queryLeft, selectedEnergyRight)
);
const filteredEnergyRight = computed(() =>
  filterEnergy(queryRight, selectedEnergyLeft)
);
</script>
