<script setup lang="ts">
import { ref } from "vue";
import { CHESSBOARD_AXIS } from "./config";

const props = defineProps<{ onSquareClick: (squareCode: string) => void }>();

const highlightedSquares = ref<string[]>([]);

const isSquareDark = (xIndex: number, yIndex: number) =>
  xIndex % 2 !== yIndex % 2;

const handleSquareClick = (squareCode: string) => {
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
</script>

<template>
  <div v-for="(xSquare, xIndex) in CHESSBOARD_AXIS.x">
    <div
      v-for="(ySquare, yIndex) in CHESSBOARD_AXIS.y"
      class="w-full h-full relative"
      :class="getSquareClasses(xIndex, yIndex)"
      @click="() => handleSquareClick(xSquare + ySquare)"
      @contextmenu.prevent="highlightedSquares?.push(xSquare + ySquare)"
    >
      <div
        class="w-full h-full md:text-lg 2xl:text-4xl text-sm"
        :class="{
          'bg-chessSquare-highlight': highlightedSquares?.includes(
            xSquare + ySquare,
          ),
        }"
      >
        <div
          v-if="ySquare === '1'"
          :class="getTextClasses(xIndex, yIndex)"
          class="absolute bottom-[3px] right-[5px]"
        >
          {{ xSquare }}
        </div>
        <div
          v-if="xSquare === 'a'"
          :class="getTextClasses(xIndex, yIndex)"
          class="absolute top-[4px] left-[5px]"
        >
          {{ ySquare }}
        </div>
      </div>
    </div>
  </div>
</template>
