<script setup>
import { ref } from "vue"

const cards = ref({
  hikari: 0,
  tane: 0,
  tan: 0,
  kasu: 0
})

const yaku = ref({
  goko:false,
  shiko:false,
  akatan:false,
  aotan:false,
  nanatan:false,
  rokutan:false,
  omoteSugawara:false,
  nomi:false,
  matukiri:false,
  inoshikacho:false,
  tsukimi:false,
  hanami:false,
  kusa:false,
  hujishima:false,
  kirishima:false,
  ameshima:false
})

const score = ref(0)

// 役と札の計算
function calcScore(){

  let cardScore =
  cards.value.hikari * 20 +
  cards.value.tane * 10 +
  cards.value.tan * 5 +
  cards.value.kasu * 1

  let yakuScore = 0

  if(yaku.value.goko) yakuScore += 200
  if(yaku.value.shiko) yakuScore += 60
  if(yaku.value.akatan) yakuScore += 40
  if(yaku.value.aotan) yakuScore += 40
  if(yaku.value.nanatan) yakuScore += 40
  if(yaku.value.rokutan) yakuScore += 30
  if(yaku.value.omoteSugawara) yakuScore += 30
  if(yaku.value.nomi) yakuScore += 30
  if(yaku.value.inoshikacho) yakuScore += 20
  if(yaku.value.matukiri) yakuScore += 20
  if(yaku.value.tsukimi) yakuScore += 20
  if(yaku.value.hanami) yakuScore += 20
  if(yaku.value.kusa) yakuScore += 20
  if(yaku.value.hujishima) yakuScore += 20
  if(yaku.value.kirishima) yakuScore += 20
  if(yaku.value.ameshima) yakuScore += 20

  score.value = cardScore + yakuScore
}

// 点数のリセット
function reset(){
  cards.value = {
    hikari:0,
    tane:0,
    tan:0,
    kasu:0
  }

  Object.keys(yaku.value).forEach(k=>{
    yaku.value[k]=false
  })

  score.value = 0
}

// 札の増減
function increase(type){
  cards.value[type]++
}

function decrease(type){
  if(cards.value[type] > 0){
    cards.value[type]--
  }
}

</script>

<template>
  <div class="container">
    <h2>花合わせ 点数計算</h2>
    <h2>札</h2>
      <div class="card-input">
        <label>光札</label>
        <div class="counter">
          <button @click="decrease('hikari')">−</button>
            <span>{{ cards.hikari }}枚</span>
          <button @click="increase('hikari')">＋</button>
        </div>
      </div>
      <div class="card-input">
        <label>タネ</label>
        <div class="counter">
          <button @click="decrease('tane')">−</button>
            <span>{{ cards.tane }}枚</span>
          <button @click="increase('tane')">＋</button>
        </div>
      </div>
      <div class="card-input">
        <label>短冊</label>
        <div class="counter">
          <button @click="decrease('tan')">−</button>
            <span>{{ cards.tan }}枚</span>
          <button @click="increase('tan')">＋</button>
        </div>
      </div>
      <div class="card-input">
        <label>カス</label>
        <div class="counter">
          <button @click="decrease('kasu')">−</button>
          <span>{{ cards.kasu }}枚</span>
          <button @click="increase('kasu')">＋</button>
        </div>
      </div>
    <h2>役</h2>
      <div class="yaku-grid">
        <label><input type="checkbox" v-model="yaku.goko">五光(200)</label>
        <label><input type="checkbox" v-model="yaku.shiko">四光(60)</label>
        <label><input type="checkbox" v-model="yaku.akatan">赤短(40)</label>
        <label><input type="checkbox" v-model="yaku.aotan">青短(40)</label>
        <label><input type="checkbox" v-model="yaku.nanatan">七短(40)</label>
        <label><input type="checkbox" v-model="yaku.rokutan">六短(30)</label>
        <label><input type="checkbox" v-model="yaku.omoteSugawara">表菅原(30)</label>
        <label><input type="checkbox" v-model="yaku.nomi">のみ(30)</label>
        <label><input type="checkbox" v-model="yaku.matukiri">松桐坊主(20)</label>
        <label><input type="checkbox" v-model="yaku.inoshikacho">猪鹿蝶(20)</label>
        <label><input type="checkbox" v-model="yaku.tsukimi">月見で一杯(20)</label>
        <label><input type="checkbox" v-model="yaku.hanami">花見で一杯(20)</label>
        <label><input type="checkbox" v-model="yaku.kusa">くさ(20)</label>
        <label><input type="checkbox" v-model="yaku.hujishima">藤島(20)</label>
        <label><input type="checkbox" v-model="yaku.kirishima">桐島(20)</label>
        <label><input type="checkbox" v-model="yaku.ameshima">雨島(20)</label>
      </div>
      <div class="buttons">
        <button @click="calcScore">点数計算</button>
        <button @click="reset">リセット</button>
      </div>
    <h2 class="score">合計 {{ score }} 点</h2>
  </div>
</template>

<style>

.container{
 max-width:400px;
 margin:auto;
 padding:20px;
 font-family:sans-serif;
}

.card-input{
 margin-bottom:10px;
 display:flex;
 justify-content:space-between;
}

.counter{
 display:flex;
 align-items:center;
 gap:10px;
}

.counter button{
 width:40px;
 height:40px;
 font-size:20px;
 display: flex;
 align-items: center;
 justify-content: center;
}

.counter span{
 font-size:20px;
 width:50px;
 text-align:center;
 margin-bottom: 10px;
}

input{
 width:20px;
 font-size:18px;
}

.yaku-grid{
 display:grid;
 grid-template-columns:1fr 1fr;
 gap:10px;
}

.yaku-grid label{
 display:flex;
 align-items:center;
 justify-content:flex-start;
 width:230px;
 gap:8px;
 text-align:left;
}

button{
 width:100%;
 font-size:18px;
 padding:12px;
 margin-bottom:10px;
}

.score{
 text-align:center;
 font-size:28px;
}

</style>