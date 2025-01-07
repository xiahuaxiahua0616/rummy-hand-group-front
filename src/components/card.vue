<script setup lang="ts">

import {ref, watch} from "vue";

const props = defineProps(['cards'])

const cardsImg = ref<string[]>([]);

cardsImg.value = []

props.cards.forEach((v: number) => {
  const suit = Math.ceil(v / 16); // 花色计算

  const rank = v % 16; // 点数计算
  if (rank != 0) {
    cardsImg.value.push(`src/assets/cards/card_${suit-1}${rank.toString(16)}.png`);
  }
})

</script>

<template>
  <div class="container">
    <div class="card" v-for="(img, index) in cardsImg" :key="index">
      <img :src="img" alt="img">
    </div>
  </div>
</template>

<style scoped>
  .container {
    display: flex;
    width: 80px;
    height: 110px;
  }

  .card:hover {
    opacity: 0.5;
  }
  .card:active {
    scale: 0.8;
  }

  img {
    width: 80px;
    height: 110px;
  }
</style>
