<script setup lang="ts">
import { VNodeRef, onMounted, onUnmounted, reactive, ref } from "vue";
import { CHESSBOARD_AXIS } from "./config.ts";

type ChessBoardStateType = {
  resizeObserver: ResizeObserver | null;
  dimension?: number;
};

defineProps<{ onSquareClick: (squareCode: string) => void }>();

const boardRef = ref<VNodeRef | null>(null);
const chessBoardState = reactive<ChessBoardStateType>({ resizeObserver: null });

onMounted(() => {
  const element = boardRef.value as HTMLElement;

  chessBoardState.resizeObserver = new ResizeObserver((entries) => {
    const entry = entries[0];
    console.log(entry.contentRect.width);

    chessBoardState.dimension = Math.min(
      entry.contentRect.width,
      entry.contentRect.height,
    );
  });

  chessBoardState.resizeObserver.observe(element);
});

onUnmounted(() => {
  chessBoardState.resizeObserver?.disconnect();
});

const isDarkSquare = (xIndex: number, yIndex: number) =>
  (xIndex % 2 !== 0 && yIndex % 2 === 0) ||
  (xIndex % 2 === 0 && yIndex % 2 !== 0);

const isLightSquare = (xIndex: number, yIndex: number) =>
  (yIndex % 2 === 0 && xIndex % 2 === 0) ||
  (yIndex % 2 !== 0 && xIndex % 2 !== 0);
</script>

<template>
  <div
    ref="boardRef"
    class="flex items-center justify-center md:p-8 p-5 w-full h-full overflow-auto"
  >
    <div
      :style="{
        width: `${chessBoardState.dimension}px`,
        height: `${chessBoardState.dimension}px`,
      }"
      class="grid grid-cols-8 grid-rows-8 grid-flow-row-dense select-none shadow"
    >
      <div v-for="(xSquare, xIndex) in CHESSBOARD_AXIS.x">
        <div
          v-for="(ySquare, yIndex) in CHESSBOARD_AXIS.y"
          class="w-full h-full relative"
          :class="{
            'bg-chessSquare-secondary': isLightSquare(xIndex, yIndex),
            'bg-chessSquare': isDarkSquare(xIndex, yIndex),
          }"
          @click="() => onSquareClick(xSquare + ySquare)"
        >
          <div
            :class="{
              'text-chessSquare': isLightSquare(xIndex, yIndex),
              'text-chessSquare-secondary': isDarkSquare(xIndex, yIndex),
            }"
            class="absolute bottom-[2px] right-[5px] text-lg"
          >
            {{ ySquare === "1" ? xSquare : "" }}
          </div>
          <div
            :class="{
              'text-chessSquare': isLightSquare(xIndex, yIndex),
              'text-chessSquare-secondary': isDarkSquare(xIndex, yIndex),
            }"
            class="absolute top-[2px] left-[5px] text-lg"
          >
            {{ xSquare === "a" ? ySquare : "" }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
