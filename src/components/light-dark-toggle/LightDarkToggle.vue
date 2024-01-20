<script setup lang="ts">
import { localStorageUtil } from "@/lib/local-storage";
import { onMounted, ref, watch } from "vue";
import { Toggle } from "radix-vue";
import { SunIcon, MoonIcon } from "lucide-vue-next";

const storageKey = "dark-mode";
const toggleState = ref<boolean>(false);

watch(toggleState, (value) => {
  const body = document.querySelector("body");

  if (value) {
    body?.classList.add("dark");
    localStorageUtil.setItem(storageKey, "dark");
  } else {
    body?.classList.remove("dark");
    localStorageUtil.removeItem(storageKey);
  }
});

onMounted(() => {
  if (localStorageUtil.getItem(storageKey)) {
    document.querySelector("body")?.classList.add("dark");
    toggleState.value = true;
  }
});
</script>

<template>
  <Toggle
    class="flex justify-center align-center"
    v-model:pressed="toggleState"
  >
    <MoonIcon class="w-5 h-5" v-if="toggleState" />
    <SunIcon class="w-5 h-5" v-else />
  </Toggle>
</template>
