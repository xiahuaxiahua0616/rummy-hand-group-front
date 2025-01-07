<script setup lang="ts">

import Card from "@/components/card.vue";
import {ref} from "vue";
import axios from "axios"

let myCards = ref<number[]>([])
let setCards = ref<number[]>([])
let setWithJokerCards = ref<number[]>([])
let sysJoker = ref<number[]>([])
let joker = ref<number[]>([])

let reqId = ref<number>(0)

let promptCards = ref<number[][]>([])

let cardsNumber = ref<number>(0)

const todoReq = async () => {
  await axios.get("http://192.168.2.173:8009/api/v1/hand/range").then(response => {
    promptCards.value = response.data.data.result

    myCards.value = response.data.data.myCards
    reqId.value += 1

    sysJoker.value = response.data.data.sysJoker ?? []

    cardsNumber.value = 0
    promptCards.value.forEach((i) => {
      cardsNumber.value += i.length
    })

  })
}

</script>

<template>
  <div class="container">
     <div class="cards">
       <div class="my-card">
         <h3>当前手牌</h3>
         <Card :cards="myCards" :key="reqId"></Card>
       </div>

       <div class="my-card">
         <h3>计算后的牌</h3>
         <div class="prompt-cards">
           <div v-for="(c, i) in promptCards" :key="reqId">
             <Card :cards="c"></Card>
           </div>
         </div>
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
        <div v-for="(c, i) in sysJoker" :key="reqId">
          <Card :cards="c"></Card>
        </div>
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

.prompt-cards {
  display: flex;
  gap: 30px;
}
</style>