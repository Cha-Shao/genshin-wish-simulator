<template>
<div class="container">
    <div class="card">
        <img src="@/assets/ui/background_horn.webp" alt="" class="leftTop">
        <img src="@/assets/ui/background_horn.webp" alt="" class="rightTop">
        <img src="@/assets/ui/background_horn.webp" alt="" class="leftBottom">
        <img src="@/assets/ui/background_horn.webp" alt="" class="rightBottom">
        <div class="hr"></div>
        <h1>欢迎你，旅行者</h1>
        <div class="hr"></div>
        <p class="content">欢迎游玩「祈愿」模拟器！</p>
        <p class="content">这是一份微薄的见面礼，请收下~</p>
        <div style="display: flex;align-items: center;justify-content: center;">
        <img src="@/assets/item/intertwined_fated.webp" alt="" class="intertwinedFate" height="30" @click="setChache()">
        <p class="content">&nbsp;x 180</p>
        </div>
        <p class="button" @click="setChache()">谢谢</p>
    </div>
</div>
</template>

<script setup>
import { ref } from 'vue'

function setChache(){
    // 加原石
    localStorage.setItem('intertwinedFate',180)
    // 设置保底
    localStorage.setItem('minimumGuarantee',0)
    // 不再弹出欢迎
    localStorage.setItem('welcomePage', true)
    // 设置金概率
    localStorage.setItem('goldRate', 6)
    // 刷新
    window.location.reload();
}
</script>

<style lang="scss" scoped>
.container{
    z-index: 99;
    position: absolute;
    height: 100vh;
    width: 100vw;
    background: #00000088;
    display: flex;
    justify-content: center;
    align-items: center;
    .card{
        position: relative;
        background: #E5E1DC;
        padding: 37px;
        text-align: center;
        border-radius: 21px;
        .hr{
            content: '';
            height: 2px;
            width: 100%;
            background: #D8D2C8;
            margin: 5px 0;
        }
        h1{
            font-size: 2.2em;
            font-weight: normal;
            margin: 7px 0;
        }
        .content{
            font-size: 1.5em;
            margin: 3px 0;
        }
        .button{
            color: white;
            background: #4A5366;
            font-size: 1.3em;
            height: 45px;
            width: 100%;
            line-height: 2;
            text-align: center;
            border-radius: 99px;
            box-shadow: 0 0 13px #00000033;
            border: 3px solid transparent;
            box-sizing: border-box;
            transition: 0.2s all;
            margin-bottom: 0;
            &:hover{
            border: 3px solid #FEECC6;
            box-shadow: 0 0 13px #F9E2B566;
            }
        }
    }
}
.leftTop{
    position: absolute;
    left: -20px;
    top: -18px;
    transform: rotate(-90deg);
}
.rightTop{
    position: absolute;
    right: -20px;
    top: -18px;
}
.leftBottom{
    position: absolute;
    left: -20px;
    bottom: -18px;
    transform: rotate(-180deg);
}
.rightBottom{
    position: absolute;
    right: -20px;
    bottom: -18px;
    transform: rotate(90deg);
}
</style>