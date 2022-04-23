<template>
  <div class="content">
    <div class="title">
      <img src="@\assets\ui\wish.svg" alt="祈愿" class="wishIcon">
      <p class="wishTitle lightFont">祈愿</p>
    </div>
    <div class="money">
      <img src="@/assets/item/intertwined-fated.png" alt="" class="intertwinedFate" height="25">
      <p class="intertwined">{{intertwinedFate}}</p>
      <img @click="supplement(10)" src="@/assets/ui/add.svg" alt="" class="add">
    </div>
    <div class="header">
      <header-button v-for="(type, i) in Configs.nowUp" :key="i"
        @click="switchMode(i, type)"
        :class="isSwitch === i ? 'switched' : ''"
        :type="type"
        :number="i"
        :isSwitch="isSwitch"
      />
    </div>
    <div class="banners"
    v-for="(banner, i) in Configs.nowUp" :key="i"
    v-show="isSwitch === i">
      <main-banners :banner="banner"/>
    </div>
    <div class="footer">
      <div class="smallButtons">
        <p>投喂</p>
        <p>详情</p>
        <p>历史记录</p>
      </div>
      <div class="wishButtons">
        <div @click="wish(1)"><p>祈愿1次</p><p
        :style="intertwinedFate < 1 ? 'color: #FF5F40':''"
        ><img src="@/assets/item/intertwined-fated.png" alt="" class="intertwinedFate" height="25">x1</p></div>
        <div @click="wish(10)"><p>祈愿10次</p><p
        :style="intertwinedFate < 10 ? 'color: #FF5F40':''"
        ><img src="@/assets/item/intertwined-fated.png" alt="" class="intertwinedFate" height="25">x10</p></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Configs from '../configs'
import HeaderButton from './HeaderButton.vue'
import MainBanners from './MainBanners.vue'
import { ref } from 'vue'
import Limits from '@/limits'

const isSwitch = ref(0)
// 首页
const homePage = ref(true)
// 放视频
const wishing = ref(false)
//结果页
const resultPage = ref(false)

let wishUper = ref(Configs.nowUp[0])
let intertwinedFate = ref(0)
let baoDi = ref(0)

let defaultResult = {
  star5: [],
  star4: [],
  star4Weapon: [],
  star3: [],
}

function switchMode(i, type) {
  isSwitch.value = i
  wishUper.value = type
  defaultResult = {
    star5: [],
    star4: [],
    star4Weapon: [],
    star3: [],
  }
  importResult()
  console.log(wishUper.value);
}

// 设置浏览器缓存
function setCache() {
  localStorage.setItem('intertwinedFate',90)
  localStorage.setItem('baoDi',0)
}
// 判断浏览器缓存是否存在
function isCache() {
  if (localStorage.getItem('intertwinedFate') && localStorage.getItem('baoDi')) {
    intertwinedFate.value = localStorage.getItem('intertwinedFate')
  } else {
    setCache()
  }
}

isCache()


let type = Configs.nowUp[0]

// 导入默认卡池内容
function importResult() {
  for (let i in Configs.defaultResult){
    Configs.defaultResult[i].forEach(item => {
      defaultResult[i].push(item)
    })
  }
}
importResult()

// 祈愿

function wish(time){
  intertwinedFate.value -= time


  if (intertwinedFate.value < 0) {
    intertwinedFate.value += time
    console.log('不够');
  }
  
  else {
    console.log(wishUper.value);
    for (let i = 0; i < Configs.defaultResult.star5.length; i++){
      defaultResult.star5.push(Limits.limits[wishUper.value].star5)
    }
    Limits.limits[wishUper.value].star4.forEach(item => {
      for (let i = 0; i < Configs.defaultResult.star4.length / 3;i++) {
        defaultResult.star4.push(item)
      }
    })
    console.log(defaultResult);
    startWish(time)
  }
  localStorage.setItem('intertwinedFate',intertwinedFate.value)
}

function startWish(time) {
  // 初始化结果
  let result = []
  // 保底
  if (time == 10) {
    result.push(defaultResult.star4Weapon[Math.floor(Math.random() * defaultResult.star4Weapon.length)])
    time = 9
  }
  // 抽卡
  for (let i = 0; i < time; i++) {
    let random = Math.floor(Math.random() * 1000)
    switch (true) {
      case (0 <= random && random < 6):
        result.push(defaultResult.star5[Math.floor(Math.random() * defaultResult.star5.length)])
        break;
      case (6 <= random && random < 130):
        result.push(defaultResult.star4[Math.floor(Math.random() * defaultResult.star4.length)])
        break;
      case (130 <= random && random < 1000):
        result.push(defaultResult.star3[Math.floor(Math.random() * defaultResult.star3.length)])
        break;
    }
  }
  // 排序result
  result.sort()
  result.reverse()
  console.log(result);
}

// 投喂
function supplement(time){
  intertwinedFate.value -= -time
  localStorage.setItem('intertwinedFate',intertwinedFate.value)
  }
</script>

<style lang="scss" scoped>
$lightfont: #F6F2EE;  
$darkfont: #3B4255;
$grayfont: #B4A08C;

.lightFont{
  color: $lightfont;
  text-shadow: 0 0 1px #3F4A4D;
}

.content{
  .title{
    position: absolute;
    display: flex;
    align-items: center;
    top: 8px;
    left: 38px;
    .wishIcon{
      height: 75px;
    }
    .wishTitle{
      margin-left: 32px;
      font-size: 1.5em;
      font-weight: lighter;
    }
  }
  .money{
    display: flex;
    align-items: center;
    position: absolute;
    top: 28px;
    right: 78px;
    color: white;
    font-size: 1.3em;
    background: #00000066;
    padding: 3px 3px 3px 7px;
    border-radius: 99px;
    border: 2px solid #ffffff44;
    p{
      margin: 0 10px;
    }
    .add{
      background: #ECE5D8;
      padding: 5px;
      height: 17px;
      border-radius: 100%;
    }
  }
  .header{
    padding: 9px 41px;
    display: flex;
    justify-content: center;
    .switched{
      background: url('../assets/ui/button-active.png') no-repeat;
      transform: scale(1.1);
      &::after{
        background: url(../assets/ui/button-active-after.png);
      }
      .character{
        transform: translateY(-6px);
      }
    }
  }
  .banners{
    display: flex;
    justify-content: center;
    // .banner{
    //   height: fit-content;
    //   width: fit-content;
    // }
  }
  .footer{
    display: flex;
    justify-content: space-around;
    align-items: flex-end;
    margin-top: 50px;
    .smallButtons{
        display: flex;
      p{
        color: #3B4255;
        background: #E3E0D5;
        font-size: 1.3em;
        height: 45px;
        width: 170px;
        margin: 0 13px;
        line-height: 2;
        text-align: center;
        border-radius: 99px;
        overflow: hidden;
        box-shadow: 0 0 13px #00000033;
        border: 3px solid #E7E6DF;
        box-sizing: border-box;
        transition: 0.1s all;
        &:hover{
          border: 3px solid white;
        box-shadow: 0 0 13px #ffffff33;
        }
      }
    }
    .wishButtons{
      display: flex;
      div{
        background: url('@/assets/ui/wish-button.png') no-repeat;
        height: 82px;
        width: 350px;
        text-align: center;
        font-size: 1.4em;
        color: #B4A08C;
        padding-top: 17px;
        margin: 0 12px;
        box-sizing: border-box;
        p{
          margin: 0;
        }
      }
    }
  }
}
</style>
