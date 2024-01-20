<script setup lang="ts">
import { VNodeRef, onMounted, onUnmounted, ref } from "vue";
import ChessBoardSquares from "./ChessBoardSquares.vue";

defineProps<{ onSquareClick: (squareCode: string) => void }>();

const boardRef = ref<VNodeRef | null>(null);
const resizeObserver = ref<ResizeObserver | null>(null);
const dimension = ref<number>();

onMounted(() => {
  const element = boardRef.value as HTMLElement;

  // resize observer to keep the chessboards dimensions fit to the screen (get discunnected on unmount)
  resizeObserver.value = new ResizeObserver((entries) => {
    const entry = entries[0];

    dimension.value = Math.min(
      entry.contentRect.width,
      entry.contentRect.height,
    );
  });

  resizeObserver.value.observe(element);
});

onUnmounted(() => {
  resizeObserver.value?.disconnect();
});
</script>

<template>
  <div
    ref="boardRef"
    class="flex items-center justify-center md:p-8 p-5 w-full h-full overflow-auto"
  >
    <div
      :style="{
        width: `${dimension}px`,
        height: `${dimension}px`,
      }"
      class="grid grid-cols-8 grid-rows-8 select-none shadow"
    >
      <ChessBoardSquares :on-square-click="onSquareClick" />
    </div>
  </div>
</template>
