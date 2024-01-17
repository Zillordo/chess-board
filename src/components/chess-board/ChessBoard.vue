<script setup lang="ts">
import { CHESSBOARD_AXIS } from "./config.ts";

defineProps<{ onSquareClick: (position: string) => void }>();

const shouldBePrimaryColor = (xIndex: number, yIndex: number) =>
  (xIndex % 2 !== 0 && yIndex % 2 === 0) ||
  (xIndex % 2 === 0 && yIndex % 2 !== 0);

const shouldBesecundaryColor = (xIndex: number, yIndex: number) =>
  (yIndex % 2 === 0 && xIndex % 2 === 0) ||
  (yIndex % 2 !== 0 && xIndex % 2 !== 0);
</script>

<template>
  <div
    class="stage rounded-md grid grid-cols-8 grid-rows-8 grid-flow-row-dense"
  >
    <div v-for="(xSquare, xIndex) in CHESSBOARD_AXIS.x">
      <div
        v-for="(ySquare, yIndex) in CHESSBOARD_AXIS.y"
        class="w-full h-full relative"
        :class="{
          'bg-primary-foreground': shouldBesecundaryColor(xIndex, yIndex),
          'bg-primary': shouldBePrimaryColor(xIndex, yIndex),
        }"
        @click="() => onSquareClick(xSquare + ySquare)"
      >
        <div
          :class="{
            'text-primary': shouldBesecundaryColor(xIndex, yIndex),
            'text-primary-foreground': shouldBePrimaryColor(xIndex, yIndex),
          }"
          class="absolute bottom-[2px] right-[5px]"
        >
          {{ ySquare === "1" ? xSquare : "" }}
        </div>
        <div
          :class="{
            'text-primary': shouldBesecundaryColor(xIndex, yIndex),
            'text-primary-foreground': shouldBePrimaryColor(xIndex, yIndex),
          }"
          class="absolute top-[2px] left-[5px]"
        >
          {{ xSquare === "a" ? ySquare : "" }}
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.stage {
  --r: 1 / 1;

  aspect-ratio: var(--r);
  width: min(100%, min(500px, 90vh * (var(--r))));
}
</style>
