<script setup lang="ts">
import { watch, reactive } from "vue";
import ChessBoard from "./components/chess-board/ChessBoard.vue";

const chessBoardState = reactive<{ sequence: string[] }>({ sequence: [] });

watch(chessBoardState, (sequence) => {
  console.log(
    "sequence: ",
    sequence.sequence.map((t) => t),
  );
});
</script>

<template>
  <div class="grid grid-cols-[minmax(200px,_auto)_1fr] h-svh w-svw">
    <div>
      {{ chessBoardState.sequence }}
    </div>
    <div class="flex items-center justify-center p-8">
      <ChessBoard
        :on-square-click="(position) => chessBoardState.sequence.push(position)"
      />
    </div>
  </div>
</template>
