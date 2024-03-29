<template>
  <TheModal v-model="showModal">
    <template #header>{{ gameResults.title }}</template>
    <template #content>
      <div class="flex flex-col">
        <p class="text-xl">{{ gameResults.message }}</p>
        <p class="mb-2">{{ profitMessage }}</p>
        <p>Dealer: {{ gameResults.dealerSum }}</p>
        <p>You: {{ gameResults.playerSum }}</p>
      </div>
    </template>
    <template #footer>
      <TheButton class="font-montserrat ml-2" @click="nextGame"
        >Next Game</TheButton
      >
    </template>
  </TheModal>
</template>

<script setup lang="ts">
import { computed, ref, watch } from "vue";
import TheModal from "@/components/misc/TheModal.vue";
import TheButton from "@/components/misc/TheButton.vue";
import { useGameStore } from "@/store/game";
import { useCreditStore } from "@/store/credits";
import { useTokenStore } from "@/store/token";

// Using stores
const gameStore = useGameStore();
const creditStore = useCreditStore();
const tokenStore = useTokenStore();

// Game result data
const showModal = ref(false); // Modal state
const profitMessage = ref(""); // Modal state
const showResult = computed(() => gameStore.getShowResults); // Show results
const playersBet = computed(() => gameStore.getPlayersBet); // Players bet
const gameResults = computed(() => gameStore.getGameResults); // Result of game

// Watcher for game result and pays out the profit
watch(showResult, (newValue) => {
  // Show modal after 2 seconds
  if (newValue) {
    setTimeout(() => {
      showModal.value = true;
    }, 1000);

    // After game is over, show modal

    // Pay out profit
    if (gameResults.value.win === "blackjack") {
      const profit = playersBet.value * 2.5;
      creditStore.addCredits(profit);
      profitMessage.value = `You won ${profit} credits!`;
    } else if (gameResults.value.win === "tie") {
      const profit = playersBet.value;
      creditStore.addCredits(profit);
      profitMessage.value = `You get your credits back: ${profit}!`;
    } else if (gameResults.value.win === true) {
      const profit = playersBet.value * 2;
      creditStore.addCredits(profit);
      profitMessage.value = `You won ${profit} credits!`;
    } else if (gameResults.value.win === false) {
      const lostCredits = playersBet.value;
      profitMessage.value = `You lose your bet: ${lostCredits} credits!`;
      if (creditStore.getCredits - gameStore.getPlayersBet < 0) {
        gameStore.setBet(0);
        tokenStore.resetTokensPlayed();
      }
    }
  }
});

// Emit next game event
const emit = defineEmits(["next-game"]);
const nextGame = () => {
  showModal.value = false;
  emit("next-game");
};
</script>
