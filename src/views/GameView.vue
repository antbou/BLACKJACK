<template>
  <div class="board">
    <div class="board-bg">
      <section id="game" class="board-section">
        <!-- Modal - for game result -->
        <GameResult @next-game="nextGame" />

        <!-- Betting window -->
        <div v-if="!gameRunning" class="mx-auto">
          <GameBet />
        </div>

        <!-- Game running -->
        <div v-else class="mx-auto">
          <!-- Result title -->
          <div
            v-if="showResult"
            class="grid justify-items-center text-4xl mb-2 md:mb-4"
          >
            <p>{{ gameResults.title }}</p>
          </div>

          <!-- Credits -->
          <TheCard class="mb-2 md:mb-4 py-2 md:py-4">
            <DisplayCredits :next-credits="credisBet" />
          </TheCard>

          <!-- Dealer -->
          <DealerBoard class="my-2 md:my-4" />

          <!-- Player -->
          <PlayerBoard class="mt-2 md:mt-4" />
        </div>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import TheCard from "@/components/misc/TheCard.vue";
import GameBet from "@/components/game/GameBet.vue";
import GameResult from "@/components/game/GameResult.vue";
import DealerBoard from "@/components/game/DealerBoard.vue";
import PlayerBoard from "@/components/game/PlayerBoard.vue";
import DisplayCredits from "@/components/game/DisplayCredits.vue";
import { useGameStore } from "@/store/game";
import { computed } from "vue";

// Using game store
const gameStore = useGameStore();

// Game states
const gameRunning = computed(() => gameStore.getGameRunning);
const showResult = computed(() => gameStore.getShowResults); // Show results
const gameResults = computed(() => gameStore.getGameResults); // Result of game

const credisBet = computed(() => gameStore.getPlayersBet);

// Starting new game
const nextGame = () => {
  gameStore.newGame();
};
</script>

<style>
@media screen and (min-width: 50px) and (min-height: 647px) {
  .board {
    @apply h-full !important;
  }
}
</style>
