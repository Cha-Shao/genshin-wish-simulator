<template>
  <welcome-toast v-if="welcomePage != 'true'"/>
  <!-- Issues! -->
  <info-toast v-if="issuesToast" title="Issues!" :content="['祈愿模拟器处于早期开发阶段','若发现问题请联系作者邮箱:','yrchashao@foxmail.com']" :buttonInfo="[{content: '好的',event: 'window.location.reload()'}]"/>
  <!-- 投喂 -->
  <info-toast v-if="feedingToast" title="投喂" :content="['祈愿模拟器处于早期开发阶段','若发现问题请联系作者邮箱:','yrchashao@foxmail.com']" :buttonInfo="[{content: 'A',event: 'window.open(\'https://mianbaoduo.com/o/bread/mbd-YpiVkpxu\')'},{content: 'B', event: ''}]"/>

  <div class="content" v-if="homePage">
    <div class="title">
      <img src="@\assets\ui\wish.svg" alt="祈愿" class="wishIcon">
      <p class="wishTitle lightFont">祈愿</p>
    </div>
    <div class="data">
      <div class="money">
        <img src="@/assets/item/minimumGuarantee.webp" alt="" class="intertwinedFate" height="25">
        <p class="intertwined">{{minimumGuarantee?minimumGuarantee:'0'}}</p>
        <img @click="setMinimumGuarantee()" src="@/assets/ui/add.svg" alt="" class="add">
      </div>
      <div class="money">
        <img src="@/assets/item/intertwined_fated.webp" alt="" class="intertwinedFate" height="25">
        <p class="intertwined">{{intertwinedFate?intertwinedFate:'0'}}</p>
        <img @click="addInterwinedFate()" src="@/assets/ui/add.svg" alt="" class="add">
      </div>
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

    <!-- banner页面在这 -->
    <div class="banners"
    v-for="(banner, i) in Configs.nowUp" :key="i"
    v-show="isSwitch === i">
      <main-banners :banner="banner"/>
    </div>

    <div class="footer">
      <div class="smallButtons">
        <p @click="playSound('button_press')" onclick="window.open('https://mianbaoduo.com/o/bread/mbd-YpiVkpxu')">投喂</p>
        <p @click="playSound('button_press')" onclick="window.open('https://space.bilibili.com/23265721')">访问作者</p>
        <p @click="playSound('button_press');issuesToast =! issuesToast">BUGS!</p>
      </div>
      <div class="wishButtons">
        <div @click="wish(1);playSound('button_press')"><p>祈愿1次</p><p
        :style="intertwinedFate < 1 ? 'color: #FF5F40':''"
        ><img src="@/assets/item/intertwined_fated.webp" alt="" class="intertwinedFate" height="25">x1</p></div>
        <div @click="wish(10);playSound('button_press')"><p>祈愿10次</p><p
        :style="intertwinedFate < 10 ? 'color: #FF5F40':''"
        ><img src="@/assets/item/intertwined_fated.webp" alt="" class="intertwinedFate" height="25">x10</p></div>
      </div>
    </div>
  </div>

  <!-- 视频页 -->
  <video v-if="wishing" :src="require('@/assets/video/'+resultVideo+'.mp4')" autoplay preload
  class="watchVideo"/>
  <p class="skip" @click="skipVideo();playSound('button_press')" v-if="wishing" style="z-index: 2;">跳过 ></p>

  <!-- 预览页 -->
  <div class="viewPage" v-if="viewPage" @click="nextItem()">
    <audio :src="require('@/assets/video/result_in.wav')" :autoplay="true"></audio>
    <p class="skip" @click="skipView();playSound('button_press')" v-show="result.length == 10">跳过 ></p>
    <div class="close" @click="closeResultPage();playSound('button_press')" v-show="result.length == 1"/>
    <div class="viewItems" v-for="(items, i) in result" :key="i">
      <div class="itemContainer" v-if="nowView === i">
        <audio :src="require('@/assets/video/result.wav')" :autoplay="true"></audio>
        <div class="info titleFadeIn">
          <img :src="require('@/assets/ui/icons/'+items.split('-')[2]+'-view.svg')" alt="" height="100">
          <div>
            <h1>{{items.split('-')[3]}}</h1>
            <img src="@/assets/ui/icons/rarity.svg" alt="⭐" class="rarity starFadeIn"
            :style="'animation-delay: '+(1.2+i/10)+'s;'"
            v-for="i in Number(items.split('-')[0])" :key="i" height="35"
            style="margin: -2px">
          </div>
        </div>
        <div style="position: relative">
          <img :src="require('@/assets/ui/viewpage/'+items.split('-')[2]+'.webp')" alt="" class="viewItemBackground itemBackground">
          <img :src="require('@/assets/characters/'+items.split('-')[0]+'-'+items.split('-')[1]+'.webp')" alt="" class="viewItem itemFadeIn">
        </div>
      </div>
    </div>
  </div>

  <!-- 结果页 -->
  <div class="resultPage" v-if="resultPage">
    <div class="close" @click="closeResultPage();playSound('button_press')"/>
    <div class="result resultfadeInRight" v-for="(results, i) in result" :key="i"
    :style="'animation-delay: '+i/10+'s;'">
      <img :src="require('@/assets/ui/icons/'+results.split('-')[2]+'-color.svg')" alt="" class="attribute" height="90">
      <div class="raritys" style="
          position: absolute;
          z-index: 2;
          top: 87%;
          left: 50%;
          transform: translateX(-50%);
          "
          >
        <img src="@/assets/ui/icons/rarity.svg" alt="⭐" class="rarity"
        v-for="i in Number(results.split('-')[0])" :key="i" height="23"
        style="margin: -2px">
      </div>
      <img :src="require('@/assets/characters/result/'+results.split('-')[0]+'-'+results.split('-')[1]+'.webp')" alt="" class="characterResult">
    </div>
  </div>
</template>

<script setup>
import Configs from '../configs'

import HeaderButton from './HeaderButton.vue'
import MainBanners from './MainBanners.vue'
import WelcomeToast from './WelcomeToast.vue'
import InfoToast from './InfoToast.vue'

import { ref, onMounted } from 'vue'
import Limits from '@/limits'


const welcomePage = ref(localStorage.getItem('welcomePage'))
const issuesToast = ref(false)
const feedingToast = ref(false)


const isSwitch = ref(0)
// 首页
const homePage = ref(true)
// 放视频
const wishing = ref(false)
// 预览页
const viewPage = ref(false)
const nowView = ref(0)
//结果页
const resultPage = ref(false)
// 默认视频
const resultVideo = ref('5starwish-single-export')

// 当前卡池
let wishUper = ref(Configs.nowUp[0])

// 初始化原石和保底
let intertwinedFate = ref(0)
let minimumGuarantee = ref(0)

// 初始化卡池结果
let result = ref([])

let defaultResult = {
  star5: [],
  star4: [],
  star3: [],
}


// isCache()
// 设置原石和保底
intertwinedFate = ref(localStorage.getItem('intertwinedFate'))
minimumGuarantee = ref(localStorage.getItem('minimumGuarantee'))

let goldRate = localStorage.getItem('goldRate')

// 声音函数
function playSound(sound){
  let playsound = new Audio(require('@/assets/video/'+sound+'.wav'))
  playsound.play()
}

// 切换卡池
function switchMode(i, type) {
  isSwitch.value = i
  wishUper.value = type
  defaultResult = {
    star5: [],
    star4: [],
    star3: [],
  }
  importResult()
}

// 默认卡池
let type = Configs.nowUp[0]


// 导入默认卡池内容
function importResult() {
  for (let i in Configs.defaultResult){
    Configs.defaultResult[i].forEach(item => {
      defaultResult[i].push(item)
    })
  }
  // 导入up
  for (let i = 0; i < Configs.defaultResult.star5.length; i++){
    defaultResult.star5.push(Limits.limits[wishUper.value].star5)
  }
  Limits.limits[wishUper.value].star4.forEach(item => {
    for (let i = 0; i < Configs.defaultResult.star4.length / 3;i++) {
      defaultResult.star4.push(item)
    }
  })
  // console.log(defaultResult);
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
    startWish(time)
  }
  localStorage.setItem('intertwinedFate',intertwinedFate.value)
}

function startWish(time) {
  // 初始化结果
  result.value = []

  // 抽卡
  for (let i = 0; i < time; i++) {
    // 设置保底
    minimumGuarantee.value -= -1
    localStorage.setItem('minimumGuarantee',minimumGuarantee.value)

    // 生成0到1000的随机数
    
    let random = Math.floor(Math.random() * 1000)
    switch (true) {
      // 大保底
      case (minimumGuarantee.value % 180 == 0):
        result.value.push(Limits.limits[wishUper.value].star5)
        break
      // 小保底
      case (minimumGuarantee.value % 90 == 0):
        result.value.push(defaultResult.star5[Math.floor(Math.random() * defaultResult.star5.length)])
        break
      // 紫保底
      case (minimumGuarantee.value % 10 == 9):
        result.value.push(defaultResult.star4[Math.floor(Math.random() * defaultResult.star4.length)])
        break;
      // 出金
      case (0 <= random && random < goldRate):
        result.value.push(defaultResult.star5[Math.floor(Math.random() * defaultResult.star5.length)])
        localStorage.setItem('minimumGuarantee',90)
        break;
      // 出紫
      case (goldRate <= random && random < 100):
        result.value.push(defaultResult.star4[Math.floor(Math.random() * defaultResult.star4.length)])
        break;
      // 出蓝
      case (100 <= random && random < 1000):
        result.value.push(defaultResult.star3[Math.floor(Math.random() * defaultResult.star3.length)])
        break;
    }
  }
  if (result.value.toString().indexOf(Limits.limits[wishUper.value].star5.split('-')[1].toString()) != -1) {
    minimumGuarantee.value = 0
    localStorage.setItem('minimumGuarantee',0)
    console.log('重置保底');
  }
  watchVideo(time, result)
}

function watchVideo(time, result) {
  if (time == 1) {
    switch (true) {
      case (result.value[0].split('-')[0] == 5):
        // 出金
        resultVideo.value = '5starwish-single-export'
        break;
      case (result.value[0].split('-')[0] == 4):
        // 出紫
        resultVideo.value = '4starwish-single-export'
        break;
      case (result.value[0].split('-')[0] == 3):
        // 出蓝
        resultVideo.value = '3starwish-single-export'
        break;
    }
  }
  else {
    switch (true) {
      case (result.value.toString().indexOf('5-') != -1):
        // 出金
        resultVideo.value = '5starwish-export'
        break;
      default:
        // 出紫
        resultVideo.value = '4starwish-export'
        break;
    }
  }
  wishing.value = true
  homePage.value = false
  // 重置预览编号
  nowView.value = 0
  // 6s后跳转
  setTimeout(() => {
    if (wishing.value) skipVideo()
  }, 6150)
}

function skipVideo(){
  wishing.value = false
  // 切换到预览页
  viewPage.value = true
  playSound('result_in')
}

// 下一个物品预览
function nextItem(){
  nowView.value -= -1
  if (nowView.value == 10) {
    skipView()
  }
  if (result.value.length == 1) closeResultPage()
}

//跳过
function skipView() {
    nowView.value = 0
    
    // 排序result
    result.value.sort()
    result.value.reverse()
    viewPage.value = false
    resultPage.value = true
}

// 加点原石
function addInterwinedFate(){
  intertwinedFate.value -= -20
  localStorage.setItem('intertwinedFate',intertwinedFate.value)
}
function setMinimumGuarantee(){
  minimumGuarantee.value = 170
  localStorage.setItem('minimumGuarantee',170)
}

function closeResultPage(){
  viewPage.value = false
  resultPage.value = false
  homePage.value = true
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
  .data{
    position: absolute;
    top: 28px;
    right: 78px;
    display: flex;
    .money{
      display: flex;
      align-items: center;
      color: white;
      font-size: 1.3em;
      background: #00000066;
      padding: 3px 3px 3px 7px;
      border-radius: 99px;
      border: 2px solid #ffffff44;
      margin: 0 10px;
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
  }
  .header{
    padding: 9px 41px;
    display: flex;
    justify-content: center;
    .switched{
      background: url('../assets/ui/buttons/button_active.webp') no-repeat;
      transform: scale(1.1);
      &::after{
        background: url(../assets/ui/buttons/button_active_after.webp);
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
    position: absolute;
    bottom: 50px;
    left: 50%;
    transform: translateX(-50%);
    width: 100vw;
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
        background: url('@/assets/ui/buttons/wish_button.webp') no-repeat;
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
.watchVideo{
  position: absolute;
  left: 0;
  top: 0;
  height: 100vh;
  width: 100vw;
  object-fit: cover;
  object-position: center center;
}

.skip{
  position: absolute;
  top: 30px;
  right: 50px;
  font-size: 1.5em;
  z-index: 3;
  color: white;
}
.viewPage{
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  background: url('@/assets/ui/result_background.webp');
  position: relative;
  .viewItems{
    .itemContainer{
      display: flex;
      justify-content: center;
      .info{
        position: absolute;
        z-index: 2;
        left: 10%;
        top: 50%;
        display: flex;
        h1{
          font-size: 4em;
          color: white;
          margin: 0;
          text-shadow: none;
          font-weight: normal;
        }
      }
      .viewItemBackground{
        position: absolute;
        height: 100vh;
        opacity: 0;
        left: -140%;
      }
      .viewItem{
        height: 100vh;
      }
    }
  }
}

.resultPage{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: url('@/assets/ui/result_background.webp');
  position: relative;
  overflow: hidden;
  .result{
    overflow: hidden;
    height: 630px;
    width: 145px;
    background: url('@/assets/ui/wishresult.svg') no-repeat;
    background-size: 100%;
    margin: 0 2px;
    flex-shrink: 0;
    position: relative;
    .attribute{
      position: absolute;
      z-index: 2;
      top: 70%;
      left: 50%;
      transform: translateX(-50%);
    }
    .raritys{
      display: flex;
    }
    .characterResult{
      width: 100%;
      height: 100%;
      object-fit: cover;
      object-position: center center;
      position: relative;
      top: 0%;
      background-size: contain;
      mask-image: url('@/assets/ui/wishresult_mask.webp');
      mask-position: 0 -1px;
      mask-repeat: no-repeat;
      filter: drop-shadow(7px 7px 0px #000000AA);
    }
  }
}

.close{
  position: absolute;
  height: 58px;
  width: 58px;
  background: url('@/assets/ui/close.webp');
  top: 58px;
  right: 58px;
}

@keyframes resultfadeInRight {
  from {
    opacity: 0;
    -webkit-transform: translate3d(25px, 0, 0);
    transform: translate3d(300px, 0, 0);
  }

  to {
    opacity: 1;
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
  }
}
.resultfadeInRight {
  animation-fill-mode: forwards;
  opacity: 0;
  animation-duration: 0.7s;
  -webkit-animation-name: resultfadeInRight;
  animation-name: resultfadeInRight;
}

@keyframes itemBackground {
  0% {
    opacity: 0;
  }
  6% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
.itemBackground {
  animation-fill-mode: forwards;
  animation-duration: 0.7s;
  animation-delay: 0.7s;
  -webkit-animation-name: itemBackground;
  animation-name: itemBackground;
}

@keyframes itemFadeIn {
  0% {
    opacity: 0;
    -webkit-transform: translate3d(25px, 0, 0);
    transform: translate3d(-70px, 0, 0) scale(5);
    filter: brightness(0);
  }
  40% {
    opacity: 1;
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(-70px, 0, 0) scale(1);
    filter: brightness(0);
  }
  60% {
    opacity: 1;
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(-70px, 0, 0) scale(1);
    filter: brightness(0);
  }
  100% {
    opacity: 1;
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0) scale(1);
    filter: brightness(1) drop-shadow(7px 7px 0px #000000BB);
  }
}
.itemFadeIn {
  animation-fill-mode: forwards;
  opacity: 0;
  animation-duration: 1.4s;
  -webkit-animation-name: itemFadeIn;
  animation-name: itemFadeIn;
  animation-timing-function: ease;
}

@keyframes titleFadeIn {
  0% {
    opacity: 0;
    -webkit-transform: translate3d(25px, 0, 0);
    transform: translate3d(70px, 0, 0);
  }
  50%{
    opacity: 0;
    -webkit-transform: translate3d(25px, 0, 0);
    transform: translate3d(70px, 0, 0);
  }
  100% {
    opacity: 1;
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
  }
}
.titleFadeIn {
  animation-fill-mode: forwards;
  opacity: 0;
  animation-duration: 0.6s;
  animation-delay: 0.6s;
  -webkit-animation-name: titleFadeIn;
  animation-name: titleFadeIn;
  animation-timing-function: ease;
}

@keyframes starFadeIn {
  from{
    opacity: 0;
    transform: scale(2);
    filter: brightness(1.3);
  }
  to{
    opacity: 1;
    transform: scale(1);
    filter: brightness(1);
  }
}
.starFadeIn {
  opacity: 0;
  animation-fill-mode: forwards;
  animation-duration: 0.6s;
  -webkit-animation-name: starFadeIn;
  animation-name: starFadeIn;
  animation-timing-function: ease;
}
</style>
