<template>
  <div class="board">
    <div class="board-bg h-full">
      <section class="board-section">
        <TheCard class="max-w-lg">
          <div class="flex flex-col">
            <div class="flex flex-col items-center mb-2">
              <p class="text-4xl md:text-5xl mb-4">Blackjack</p>
            </div>
            <div class="flex flex-col mb-4">
              <p>Blackjack is a small blackjack clone.</p>
              <p>
                You can find the rules of the game by clicking the button below.
              </p>
            </div>
            <div class="flex flex-col mb-4">
              <p>It was built using Vue3 and Tailwind for the design.</p>
              <p>
                You can find the source code on Github by clicking the button in
                the top corner.
              </p>
            </div>
            <div class="flex flex-col mb-4">
              <p>
                The game is still in development, so there are still some
                features missing.
              </p>
              <p>
                If you have any suggestions or find any bugs, please let me know
                via Github.
              </p>
            </div>
            <div class="grid grid-cols-2 gap-4 mt-4">
              <TheButton
                class="font-montserrat w-full"
                @click="$router.push({ name: 'game' })"
              >
                New Game
              </TheButton>
              <TheButton
                class="font-montserrat w-full"
                @click="$router.push({ name: 'rules' })"
                type="secondary"
              >
                Rules
              </TheButton>
            </div>
          </div>
        </TheCard>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import TheCard from "@/components/misc/TheCard.vue";
import TheButton from "@/components/misc/TheButton.vue";
</script>

<style>
@media screen and (min-width: 326px) and (min-height: 575px),
  screen and (min-width: 224px) and (min-height: 720px) {
  body,
  html,
  #app,
  .board-bg {
    @apply h-full;
  }
}
</style>
