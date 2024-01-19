<script setup lang="ts">
import { computed, ref } from "vue";
import ChessBoard from "@/components/chess-board/ChessBoard.vue";
import MovesSidebar from "@/components/moves-sidebar/MovesSidebar.vue";

const sequence = ref<string[]>([]);

const movesFromSequence = computed(() => {
  const arr = sequence.value;
  const tuple = [];
  for (let i = 0; i < arr.length; i += 2) {
    tuple.push([arr[i], arr[i + 1]]);
  }
  return tuple;
});
</script>

<template>
  <div
    class="grid grid-rows-[1fr__minmax(300px,_0.2fr)] md:grid-rows-1 lg:grid-cols-[1fr_minmax(400px,_0.2fr)] md:grid-cols-[1fr_minmax(300px,_0.2fr)] h-full"
  >
    <ChessBoard :on-square-click="(squareCode) => sequence.push(squareCode)" />

    <MovesSidebar :moves="movesFromSequence" />
  </div>
</template>
