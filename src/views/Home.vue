<script setup lang="ts">

import Card from "@/components/card.vue";
import {ref} from "vue";
import axios from "axios"

let myCards = ref<number[]>([])
let pureCards = ref<number[]>([])
let pureWithCardsCards = ref<number[]>([])
let setCards = ref<number[]>([])
let setWithJokerCards = ref<number[]>([])
let invalidCards = ref<number[]>([])
let sysJoker = ref<number[]>([])
let joker = ref<number[]>([])

let cardsNumber = ref<number>(0)

const todoReq = async () => {
  await axios.get("http://192.168.2.173:8080").then(response => {
    myCards.value = response.data.myCards ?? []
    pureCards.value = response.data.pure ?? []
    pureWithCardsCards.value = response.data.pureWithJoker ?? []
    setCards.value = response.data.set ?? []
    setWithJokerCards.value = response.data.setWithJoker ?? []
    invalidCards.value = response.data.invalid ?? []
    sysJoker.value = response.data.sysJoker ?? []
    joker.value = response.data.joker ?? []

    cardsNumber.value = 0

    pureCards.value.forEach((i) => {
      if (i != 0) {
        cardsNumber.value += 1
      }
    })

    pureWithCardsCards.value.forEach((i) => {
      if (i != 0) {
        cardsNumber.value += 1
      }
    })

    setCards.value.forEach((i) => {
      if (i != 0) {
        cardsNumber.value += 1
      }
    })

    setWithJokerCards.value.forEach((i) => {
      if (i != 0) {
        cardsNumber.value += 1
      }
    })

    invalidCards.value.forEach((i) => {
      if (i != 0) {
        cardsNumber.value += 1
      }
    })

    joker.value.forEach((i) => {
      if (i != 0) {
        cardsNumber.value += 1
      }
    })

  })
}

</script>

<template>
  <div class="container">
     <div class="cards">
       <div class="my-card">
         <h3>当前手牌</h3>
         <Card :cards="myCards"></Card>
       </div>

       <div class="my-card">
         <h3>无效牌</h3>
         <Card :cards="invalidCards"></Card>
       </div>

       <div class="my-card">
         <h3>纯顺子</h3>
         <Card :cards="pureCards"></Card>
       </div>

       <div class="my-card">
         <h3>带Joker的顺子</h3>
         <Card :cards="pureWithCardsCards"></Card>
       </div>

       <div class="my-card">
         <h3>返回牌的数量是</h3>
         <p class="my-cards-num">{{ cardsNumber }}</p>
       </div>


     </div>

    <div class="cards-2">
      <div class="my-card">
        <h3>刻子</h3>
        <Card :cards="setCards"></Card>
      </div>

      <div class="my-card">
        <h3>带Joker的刻子</h3>
        <Card :cards="setWithJokerCards"></Card>
      </div>

      <div class="my-card">
        <h3>剩余的Joker</h3>
        <Card :cards="joker"></Card>
      </div>

      <div class="my-card">
        <h3>当前系统的Joker</h3>
        <Card :cards="sysJoker"></Card>
      </div>
    </div>

    <div class="option">
      <el-button type="primary" @click="todoReq">请求</el-button>
    </div>
   </div>

</template>

<style scoped>
.container {
  width: 100%;
  display: flex;
  flex-direction: row;
}

.cards {
  width: 60%;
  display: flex;
  flex-direction: column;
  margin: 10px;
}

.cards-2 {
  width: 30%;
  display: flex;
  flex-direction: column;
  margin: 10px;
}

.my-cards-num {
  font-size: 24px;
  font-weight: bold;
}
</style>