<script setup lang="ts">
import Calc from "@/components/Calc.vue";
</script>

<template>
  <div class="w-screen h-screen bg-gradient-to-br from-green-400 to-blue-400">
    <div class="pt-10">
      <p class="text-6xl font-bold text-center mb-5">MCEC</p>
      <p class="text-2xl font-normal text-center mb-10">
        A simple Minecraft Energy Converter for mods.
      </p>
    </div>
    <Calc />
  </div>
</template>
