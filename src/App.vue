<script setup>
import { ref, computed } from "vue"

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

// 役リスト
const yakuList = [
  {key:"goko", name:"五光", score:200},
  {key:"shiko", name:"四光", score:60},
  {key:"akatan", name:"赤短", score:40},
  {key:"aotan", name:"青短", score:40},
  {key:"nanatan", name:"七短", score:40},
  {key:"rokutan", name:"六短", score:30},
  {key:"omoteSugawara", name:"表菅原", score:30},
  {key:"nomi", name:"のみ", score:30},
  {key:"matukiri", name:"松桐坊主", score:20},
  {key:"inoshikacho", name:"猪鹿蝶", score:20},
  {key:"tsukimi", name:"月見で一杯", score:20},
  {key:"hanami", name:"花見で一杯", score:20},
  {key:"kusa", name:"くさ", score:20},
  {key:"hujishima", name:"藤島", score:20},
  {key:"kirishima", name:"桐島", score:20},
  {key:"ameshima", name:"雨島", score:20}
]

// 役と札の計算
const score = computed(() => {

  const cardScore =
    cards.value.hikari * 20 +
    cards.value.tane * 10 +
    cards.value.tan * 5 +
    cards.value.kasu

  const yakuScore = yakuList.reduce((sum,y)=>{
    return yaku.value[y.key] ? sum + y.score : sum
  },0)

  return cardScore + yakuScore
})


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
    <!-- <h2>花合わせ 点数計算</h2> -->
    <div class="section-title">札</div>
      <div class="card-input">
        <label>光札</label>
        <div class="counter">
          <button @pointerdown="decrease('hikari')">−</button>
            <span>{{ cards.hikari }}枚</span>
          <button @pointerdown="increase('hikari')">＋</button>
        </div>
      </div>
      <div class="card-input">
        <label>タネ</label>
        <div class="counter">
          <button @pointerdown="decrease('tane')">−</button>
            <span>{{ cards.tane }}枚</span>
          <button @pointerdown="increase('tane')">＋</button>
        </div>
      </div>
      <div class="card-input">
        <label>短冊</label>
        <div class="counter">
          <button @pointerdown="decrease('tan')">−</button>
            <span>{{ cards.tan }}枚</span>
          <button @pointerdown="increase('tan')">＋</button>
        </div>
      </div>
      <div class="card-input">
        <label>カス</label>
        <div class="counter">
          <button @pointerdown="decrease('kasu')">−</button>
          <span>{{ cards.kasu }}枚</span>
          <button @pointerdown="increase('kasu')">＋</button>
        </div>
      </div>
    <div class="section-title">役</div>
      <div class="yaku-grid">
        <div
          v-for="y in yakuList"
          :key="y.key"
          class="yaku-card"
          :class="{active:yaku[y.key]}"
          @click="yaku[y.key] = !yaku[y.key]"
        >
          <span class="yaku-name">{{ y.name }}</span>
          <span class="yaku-score">{{ y.score }}</span>
        </div>
      </div>
    <div class="score">合計 {{ score }} 点</div>
      <div class="buttons">
        <button @click="reset">得点リセット</button>
      </div>
  </div>
</template>

<style>
*{
  touch-action: manipulation;
}

.container{
 max-width:400px;
 margin:auto;
 padding:0px;
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
 width:30px;
 height: 30px;
 font-size:18px;
}

.yaku-grid{
 display:grid;
 grid-template-columns:1fr 1fr 1fr 1fr;
 gap:10px;

 margin-left:-20px;
 margin-right:-20px;
}

.yaku-grid label{
 display:flex;
 align-items:center;
 justify-content:flex-start;
 width:100%;
 gap:8px;
 text-align:left;
}

.section-title{
 font-size:20px;
 font-weight:bold;
 margin:10px 0 6px;
}

button{
 width:100%;
 font-size:18px;
 padding:12px;
 margin:10px 0;
}

.score{
 text-align:center;
 font-size:28px;
 font-weight: bold;
 margin-top: 20px;
}

.yaku-card{
 border:1px solid #ccc;
 border-radius:10px;
 padding:0;
 text-align:center;
 font-size:14px;
 cursor:pointer;
 width: 80px;
 height: 50px;
 display: flex;
 flex-direction:column;
 justify-content:center;
 align-items:center;
}

.yaku-card.active{
 background:#ffe9a8;
 border:1px solid orange;
 font-weight:bold;
}

</style>