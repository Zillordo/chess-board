<script setup lang="ts">
import ThemeSelect from "@/components/theme-select/ThemeSelect.vue";
import LightDarkToggle from "@/components/light-dark-toggle/LightDarkToggle.vue";
import { ref, watch } from "vue";

const props = defineProps<{ moves: string[][] }>();

const scrollElementRef = ref<HTMLElement | null>(null);

watch(
  () => props.moves.at(-1),
  () => {
    const ref = scrollElementRef.value;
    if (ref) {
      ref.scrollTo(0, ref.scrollHeight);
    }
  },
  { deep: true },
);
</script>

<template>
  <div
    class="w-full h-full flex flex-col justify-between border-l bg-primary-foreground"
  >
    <div
      id="scroller"
      ref="scrollElementRef"
      class="mb-5 overflow-auto h-full md:pt-8 pt-5"
    >
      <div
        v-for="(record, index) in moves"
        class="grid py-1 px-4 grid-cols-[3ch_0.2fr_0.1fr]"
        :class="{
          'bg-primary-foreground': index % 2 === 0,
          'bg-secondary': index % 2 !== 0,
        }"
      >
        <div>{{ index + 1 }}.</div>
        <div
          :class="{
            underline: !record.at(1) && moves.length - 1 === index,
          }"
        >
          {{ record.at(0) }}
        </div>
        <div>
          <div
            :class="{
              underline: moves.length - 1 === index,
            }"
          >
            {{ record.at(1) }}
          </div>
        </div>
      </div>
      <div id="anchor"></div>
    </div>

    <div class="px-4 py-5 bg-muted border-t flex">
      <ThemeSelect />
      <LightDarkToggle class="p-2 pr-0" />
    </div>
  </div>
</template>

<style>
#scroller * {
  overflow-anchor: none;
}

#anchor {
  overflow-anchor: auto;
  height: 1px;
}
</style>
