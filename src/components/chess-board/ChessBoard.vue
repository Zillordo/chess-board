<script setup lang="ts">
import { VNodeRef, onMounted, onUnmounted, ref } from "vue";
import { CHESSBOARD_AXIS } from "./config.ts";

const props = defineProps<{ onSquareClick: (squareCode: string) => void }>();

const boardRef = ref<VNodeRef | null>(null);
const resizeObserver = ref<ResizeObserver | null>(null);
const highlightedSquares = ref<string[]>([]);
const dimension = ref<number>();

const isSquareDark = (xIndex: number, yIndex: number) =>
  xIndex % 2 !== yIndex % 2;

const onSquareClick = (squareCode: string) => {
  props.onSquareClick(squareCode);
  highlightedSquares.value = [];
};

const getSquareClasses = (xIndex: number, yIndex: number) => {
  const isDark = isSquareDark(xIndex, yIndex);
  return {
    "bg-chessSquare-secondary": isDark,
    "bg-chessSquare": !isDark,
  };
};

const getTextClasses = (xIndex: number, yIndex: number) => {
  const isDark = isSquareDark(xIndex, yIndex);
  return {
    "text-chessSquare": isDark,
    "text-chessSquare-secondary": !isDark,
  };
};

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
      class="grid grid-cols-8 grid-rows-8 grid-flow-row-dense select-none shadow"
    >
      <div v-for="(xSquare, xIndex) in CHESSBOARD_AXIS.x">
        <div
          v-for="(ySquare, yIndex) in CHESSBOARD_AXIS.y"
          class="w-full h-full relative"
          :class="getSquareClasses(xIndex, yIndex)"
          @click="() => onSquareClick(xSquare + ySquare)"
          @contextmenu.prevent="highlightedSquares?.push(xSquare + ySquare)"
        >
          <div
            class="w-full h-full"
            :class="{
              'bg-chessSquare-highlight': highlightedSquares?.includes(
                xSquare + ySquare,
              ),
            }"
          >
            <div
              v-if="ySquare === '1'"
              :class="getTextClasses(xIndex, yIndex)"
              class="absolute bottom-[3px] right-[5px] text-lg"
            >
              {{ xSquare }}
            </div>
            <div
              v-if="xSquare === 'a'"
              :class="getTextClasses(xIndex, yIndex)"
              class="absolute top-[4px] left-[5px] text-lg"
            >
              {{ ySquare }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
