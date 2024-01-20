<script setup lang="ts">
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";
import { onMounted, ref, watch } from "vue";
import { localStorageUtil } from "@/lib/local-storage";

const storageKey = "board-theme";
const theme = ref<string>("");

watch(theme, (value, prevValue) => {
  const body = document.querySelector("body");

  if (prevValue) {
    body?.classList.toggle(prevValue);
    localStorageUtil.removeItem(storageKey);
  }

  body?.classList.toggle(value);
  localStorageUtil.setItem(storageKey, value);
});

onMounted(() => {
  const storageTheme = localStorageUtil.getItem<string>(storageKey);

  if (storageTheme) {
    theme.value = storageTheme;
  }
});
</script>

<template>
  <Select v-model="theme">
    <SelectTrigger class="w-full">
      <SelectValue placeholder="Select a chess board theme" />
    </SelectTrigger>
    <SelectContent>
      <SelectGroup>
        <SelectItem value="default">Default</SelectItem>
        <SelectItem value="chess-graphite">Graphite</SelectItem>
        <SelectItem value="chess-gold">Gold</SelectItem>
        <SelectItem value="chess-alpine-green">Alpine Green</SelectItem>
        <SelectItem value="chess-starlight">Starlight</SelectItem>
        <SelectItem value="chess-red">Red</SelectItem>
      </SelectGroup>
    </SelectContent>
  </Select>
</template>
