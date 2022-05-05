<template>
<div class="container">
    <div class="card fadeIn">
        <img src="@/assets/ui/background_horn.webp" alt="" class="leftTop">
        <img src="@/assets/ui/background_horn.webp" alt="" class="rightTop">
        <img src="@/assets/ui/background_horn.webp" alt="" class="leftBottom">
        <img src="@/assets/ui/background_horn.webp" alt="" class="rightBottom">
        <div class="hr"></div>
        <h1>{{title}}</h1>
        <div class="hr"></div>
        <div class="part" v-for="(parts, i) in content" :key="i">
            <p class="content" v-for="(contents, j) in parts" :key="j">
                {{contents}}
            </p>
        </div>
        <div class="buttonContainer">
            <p class="button" v-for="(buttonStr, i) in buttonInfo" :key="i" :onclick="buttonStr.event">{{buttonStr.content}}</p>
        </div>
    </div>
</div>
</template>

<script setup>
const props = defineProps({
    title: String,
    content: Object,
    buttonInfo: Object,
    issuesToast: Boolean
})

function closePage(menu){
    debugger
    if (menu === 'issuesToast'){
        issuesToast =! issuesToast
    }
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
        .part{
            margin: 21px 0;
            .content{
                font-size: 1.5em;
                margin: 3px 0;
            }
        }
        .buttonContainer{
            display: flex;
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
                margin: 0 7px;
                min-width: 200px;
                &:hover{
                border: 3px solid #FEECC6;
                box-shadow: 0 0 13px #F9E2B566;
                }
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

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: scale(0.5);
    }
    to {
        opacity: 1;
        transform: scale(1);
    }
}

.fadeIn{
    animation: fadeIn 0.1s;
}
</style>