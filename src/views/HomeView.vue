<template>
  <div class="content" v-if="homePage">
    <div class="title">
      <img src="@\assets\ui\wish.svg" alt="祈愿" class="wishIcon">
      <p class="wishTitle lightFont">祈愿</p>
    </div>
    <div class="data">
      <div class="money">
        <img src="@/assets/item/minimumGuarantee.png" alt="" class="intertwinedFate" height="25">
        <p class="intertwined">{{minimumGuarantee}}</p>
        <img @click="setMinimumGuarantee()" src="@/assets/ui/add.svg" alt="" class="add">
      </div>
      <div class="money">
        <img src="@/assets/item/intertwined-fated.png" alt="" class="intertwinedFate" height="25">
        <p class="intertwined">{{intertwinedFate}}</p>
        <img @click="supplement(10)" src="@/assets/ui/add.svg" alt="" class="add">
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

  <!-- 视频页 -->
  <video v-if="wishing" :src="'http://localhost:25565/video/'+resultVideo+'.mp4'" autoplay
  class="watchVideo"/>

  <!-- 结果页 -->
  <div class="resultPage" v-if="resultPage">
    <div class="result resultfadeInRight" v-for="(results, i) in result" :key="i"
    :style="'animation-delay: '+i*2/10+'s;'">
      <img :src="'http://localhost:25565/characters/'+results.split('_')[0]+'_'+results.split('_')[1]+'.png'" alt="">
    </div>
  </div>
</template>

<script setup>
import Configs from '../configs'
import HeaderButton from './HeaderButton.vue'
import MainBanners from './MainBanners.vue'
import { ref } from 'vue'
import Limits from '@/limits'


let goldRank = 6


const isSwitch = ref(0)
// 首页
const homePage = ref(true)
// 放视频
const wishing = ref(false)
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

// 第一次设置原石和保底
function setCache() {
  localStorage.setItem('intertwinedFate',90)
  localStorage.setItem('minimumGuarantee',0)
}
// 判断浏览器缓存是否存在
function isCache() {
  if (localStorage.getItem('intertwinedFate') && localStorage.getItem('minimumGuarantee')) {
    intertwinedFate.value = localStorage.getItem('intertwinedFate')
  } else {
    setCache()
  }
}

isCache()
// 设置原石和保底
intertwinedFate = ref(localStorage.getItem('intertwinedFate'))
minimumGuarantee = ref(localStorage.getItem('minimumGuarantee'))



// 切换卡池
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
  console.log(defaultResult);
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
    // 导入up
    for (let i = 0; i < Configs.defaultResult.star5.length; i++){
      defaultResult.star5.push(Limits.limits[wishUper.value].star5)
    }
    Limits.limits[wishUper.value].star4.forEach(item => {
      for (let i = 0; i < Configs.defaultResult.star4.length / 3;i++) {
        defaultResult.star4.push(item)
      }
    })
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
      case (minimumGuarantee.value % 10 == 0):
        result.value.push(defaultResult.star4[Math.floor(Math.random() * defaultResult.star4.length)])
        break;
      // 出金
      case (0 <= random && random < goldRank):
        result.value.push(defaultResult.star5[Math.floor(Math.random() * defaultResult.star5.length)])
        break;
      // 出紫
      case (6 <= random && random < 130):
        result.value.push(defaultResult.star4[Math.floor(Math.random() * defaultResult.star4.length)])
        break;
      // 出蓝
      case (130 <= random && random < 1000):
        result.value.push(defaultResult.star3[Math.floor(Math.random() * defaultResult.star3.length)])
        break;
    }
  }
  // 排序result
  result.value.sort()
  result.value.reverse()
  watchVideo(time, result)
}

function watchVideo(time, result) {
  console.log(time, result);
  if (time == 1) {
    console.log('单发');
    switch (true) {
      case (result.value[0].split('_')[0] == 5):
        // 出金
        resultVideo.value = '5starwish-single-export'
        break;
      case (result.value[0].split('_')[0] == 4):
        // 出紫
        resultVideo.value = '4starwish-single-export'
        break;
      case (result.value[0].split('_')[0] == 3):
        // 出蓝
        resultVideo.value = '3starwish-single-export'
        break;
    }
  }
  else {
    console.log('十连');
    switch (true) {
      case (result.value[0].split('_')[0] == 5):
        // 出金
        console.log('出金');
        resultVideo.value = '5starwish-export'
        break;
      case (result.value[0].split('_')[0] == 4):
        // 出紫
        resultVideo.value = '4starwish-export'
        break;
    }
  }
  wishing.value = true
  homePage.value = false
  // 6s后停止播放
  setTimeout(() => {
    wishing.value = false
    // 切换到结果页
    resultPage.value = true
  }, 6000)
}

// 投喂
function supplement(time){
  intertwinedFate.value -= -time
  localStorage.setItem('intertwinedFate',intertwinedFate.value)
}
function setMinimumGuarantee(){
  minimumGuarantee.value = 170
  localStorage.setItem('minimumGuarantee',170)
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
.watchVideo{
  position: absolute;
  left: 0;
  top: 0;
  height: 100vh;
  width: 100vw;
  object-fit: cover;
  object-position: center center;
}
.resultPage{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  .result{
    overflow: hidden;
    height: 630px;
    width: 145px;
    background: url('http://localhost:25565/ui/wishresult.svg') no-repeat;
    background-size: 100%;
    margin: 0 10px;
    flex-shrink: 0;
    img{
      width: 220%;
      height: 220%;
      object-fit: cover;
      object-position: center center;
      position: relative;
      top: -20%;
      background-size: contain;
      mask-image: url('@/assets/ui/wishresult-mask.png');
      mask-position: 0 125px;
      mask-repeat: no-repeat;
    }
  }
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
</style>
