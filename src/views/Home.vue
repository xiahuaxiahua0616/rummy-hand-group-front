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

const todoReq = async (version: 'v1' | 'v2') => {
  const url = version === 'v1'
      ? 'http://124.71.76.79:8009/api/v1/hand/range'
      : 'http://124.71.76.79:8009/api/v2/hand/range'

  try {
    const response = await axios.get(url)

    promptCards.value = response.data.data.result
    myCards.value = response.data.data.myCards
    sysJoker.value = response.data.data.sysJoker ?? []
    reqId.value += 1

    cardsNumber.value = promptCards.value.reduce((sum, item: any[]) => sum + item.length, 0)
  } catch (error) {
    console.error(`请求${version}失败：`, error)
  }
}

// 0x01 0x02 0x03 0x04 0x05 0x06 0x07 0x08 0x09 0x0a 0x0b 0x0c 0x0d // 方片
// 0x11 0x12 0x13 0x14 0x15 0x16 0x17 0x18 0x19 0x1a 0x1b 0x1c 0x1d // 梅花
// 0x21 0x22 0x23 0x24 0x25 0x26 0x27 0x28 0x29 0x2a 0x2b 0x2c 0x2d // 红桃
// 0x31 0x32 0x33 0x34 0x35 0x36 0x37 0x38 0x39 0x3a 0x3b 0x3c 0x3d // 黑桃
// 0x4e 0x4f 大小王

/*
* | 十六进制 | 十进制 | 牌面   |
| ---- | --- | ---- |
| 0x01 | 1   | 方片A  |
| 0x02 | 2   | 方片2  |
| 0x03 | 3   | 方片3  |
| 0x04 | 4   | 方片4  |
| 0x05 | 5   | 方片5  |
| 0x06 | 6   | 方片6  |
| 0x07 | 7   | 方片7  |
| 0x08 | 8   | 方片8  |
| 0x09 | 9   | 方片9  |
| 0x0a | 10  | 方片10 |
| 0x0b | 11  | 方片J  |
| 0x0c | 12  | 方片Q  |
| 0x0d | 13  | 方片K  |
| 0x11 | 17  | 梅花A  |
| 0x12 | 18  | 梅花2  |
| 0x13 | 19  | 梅花3  |
| 0x14 | 20  | 梅花4  |
| 0x15 | 21  | 梅花5  |
| 0x16 | 22  | 梅花6  |
| 0x17 | 23  | 梅花7  |
| 0x18 | 24  | 梅花8  |
| 0x19 | 25  | 梅花9  |
| 0x1a | 26  | 梅花10 |
| 0x1b | 27  | 梅花J  |
| 0x1c | 28  | 梅花Q  |
| 0x1d | 29  | 梅花K  |
| 0x21 | 33  | 红桃A  |
| 0x22 | 34  | 红桃2  |
| 0x23 | 35  | 红桃3  |
| 0x24 | 36  | 红桃4  |
| 0x25 | 37  | 红桃5  |
| 0x26 | 38  | 红桃6  |
| 0x27 | 39  | 红桃7  |
| 0x28 | 40  | 红桃8  |
| 0x29 | 41  | 红桃9  |
| 0x2a | 42  | 红桃10 |
| 0x2b | 43  | 红桃J  |
| 0x2c | 44  | 红桃Q  |
| 0x2d | 45  | 红桃K  |
| 0x31 | 49  | 黑桃A  |
| 0x32 | 50  | 黑桃2  |
| 0x33 | 51  | 黑桃3  |
| 0x34 | 52  | 黑桃4  |
| 0x35 | 53  | 黑桃5  |
| 0x36 | 54  | 黑桃6  |
| 0x37 | 55  | 黑桃7  |
| 0x38 | 56  | 黑桃8  |
| 0x39 | 57  | 黑桃9  |
| 0x3a | 58  | 黑桃10 |
| 0x3b | 59  | 黑桃J  |
| 0x3c | 60  | 黑桃Q  |
| 0x3d | 61  | 黑桃K  |
| 0x4e | 78  | 小王   |
| 0x4f | 79  | 大王   |

* */

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
      <el-button type="primary" @click="() => todoReq('v1')">请求v1</el-button>
      <el-button type="primary" @click="() => todoReq('v2')">请求v2</el-button>
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