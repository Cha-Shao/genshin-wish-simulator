<template>
    <div class="container animated mediumfadeInRight">
        <div class="remarks mediumfadeInRight" :style="'background: #'+Limits.limits[banner].color+';'">角色活动祈愿</div>
        <div class="banner"
            :style="'background: url('+require('@/assets/ui/banners/banner_'+Limits.limits[banner].attribute+'.png')+')'"
        >
            <div class="contents smallfadeInRight">
                <div class="title textOutline">
                    <span :style="'color: #'+Limits.limits[banner].color+';'">
                        {{Limits.limits[banner].title.slice(0,2)}}
                    </span>
                    <span>{{Limits.limits[banner].title.slice(2)}}</span>
                </div>
                <div class="describe">
                    <p class="rateUp textOutline">获取概率提升！</p>
                    <p :style="'background-color: #'+Limits.limits[banner].color+'dd;'" class="mustHave">
                    <img src="@/assets/ui/icons/star.svg" alt="×" style="height: 1em;margin-right: 7px;">
                    每十次祈愿必出四星或以上物品。</p>
                    <p  class="more textOutline">活动限定五星角色仅在UP期间，通过对应祈愿获得。具体内容查看【详情】</p>
                </div>
                <div class="time textOutline">
                    <p>剩余时间</p>
                    <p>∞</p>
                </div>
            </div>
            <div class="upCharacter mediumfadeInRight">
                <img :src="require('@/assets/characters/5-'+Limits.limits[banner].star5.split('-')[1]+'.png')" alt="" class="up">
            </div>
            <div class="characterInfo">
                <div class="infoTitle">
                    <img :src="require('@/assets//ui/icons/'+Limits.limits[banner].attribute+'-color.webp')" alt="">
                    <div class="title darkTextOutLine">{{Limits.limits[banner].name}}</div>
                </div>
                <div class="rarityBG">
                    <img src="@/assets/ui/icons/rarity.svg" alt="⭐" class="rarity"
                    v-for="i in 5" :key="i">
                </div>
                <p class="slogan">{{Limits.limits[banner].slogan}}</p>
            </div>
            <div class="subCharacter smallfadeInRight">
                <img v-for="(character, i) in Limits.limits[banner].star4" :key="i" :src="require('@/assets//characters/withoutBackground/4-'+character.split('-')[1]+'.png')" :alt="character" :class="'character'+i">
            </div>
        </div>
    </div>
</template>

<script>
export default{
    name: 'HeaderButton',
    props: {
      banner: String,
    }
}
</script>
<script setup>
import Limits from '../limits'

const fire = 'EC4922'
const water = '498FCC'
const wind = '359697'
const electric = '6956C2'
const ice = '35AACC'
const stone = 'CC9047'
</script>

<style lang="scss" scoped>
$outline: #F0F0EE;
$darkouline: #3A4041;

.darkfont{
    color: #565656;
}
.textOutline{
    text-shadow: 1px 1px 0 $outline,
                1px -1px 0 $outline,
                -1px 1px 0 $outline,
                -1px -1px 0 $outline;
}
.darkTextOutLine{
    text-shadow: 1px 1px 0 $darkouline,
                1px -1px 0 $darkouline,
                -1px 1px 0 $darkouline,
                -1px -1px 0 $darkouline;
}

.container{
    position: relative;
    transform-style: preserve-3d;
    perspective: 5px;
    .remarks{
        padding: 5px 13px;
        padding-right: 21px;
        font-size: 1.5em;
        color: white;
        border-radius: 100px 0 200px 100px;
        position: absolute;
        left: -8px;
        top: -5px;
        width: fit-content;
        display: inline-block;
        margin-bottom: -12px;
        z-index: 2;
    }
    .banner{
        position: relative;
        border-radius: 9px;
        overflow: hidden;
        width: 1295px;
        height: 670px;
        background-size: 100% 100% !important;
        display: flex;
        z-index: 1;
        .contents{
            z-index: 3;
            padding: 52px;
            position: relative;
            flex-shrink: 0;
            animation-duration: 0.7s;
            .title{
                font-size: 4.75em;
                margin-top: 30px;
            }
            .describe{
                position: relative;
                .rateUp{
                    font-size: 1.9em;
                    margin: 0;
                    margin-top: 60px;
                }
                .mustHave{
                    color: white;
                    font-size: 1.5em;
                    padding: 3px 13px;
                    filter: brightness(1.1);
                    margin: 7px 0;
                    display: flex;
                    align-items: center;
                    width: fit-content;
                    border-radius: 1px;
                    overflow: hidden;
                }
                .more{
                    font-size: 1.5em;
                    max-width: 440px;
                    margin: 3px 0;
                }
                &::after{
                    content: '';
                    height: 100%;
                    width: 7px;
                    position: absolute;
                    left: -40px;
                    top: 0;
                    background: #565656;
                    border-radius: 2px;
                    overflow: hidden;
                }
            }
            .time{
                margin-top: 170px;
                font-size: 1.4em;
                p{
                    margin: 7px 0;
                }
            }
        }
        .upCharacter{
            position: absolute;
            animation-duration: 0.7s;
            .up{
                position: relative;
                width: 160%;
                left: -410px;
                top: -80px;
            }
        }
        .characterInfo{
            position: absolute;
            top: 64%;
            left: 48%;
            z-index: 2;
            .infoTitle{
                display:flex;
                align-items: flex-start;
                img{
                    max-height: 85px;
                }
                .title{
                    color: white;
                    font-size: 4.3em;
                    &::after{
                        content: 'UP!';
                        color: #FFFF59;
                        text-shadow: 1px 1px 0 #C98039,
                                    1px -1px 0 #C98039,
                                    -1px 1px 0 #C98039,
                                    -1px -1px 0 #C98039,
                                    0 0 15px rgba($color: #C98039, $alpha: 1);
                        font-size: 0.35em;
                        position: absolute;
                        top: 0;
                    }
                }
            }
            .rarityBG{
                z-index: -1;
                position: relative;
                left: 45px;
                top: -50px;
                background: rgba($color: #000000, $alpha: 0.6);
                width: 80%;
                padding: 40px 0 3px 45px;
                .rarity{
                    height: 1.7em;
                }
            }
            .slogan{
                color: #CAB58A;
                background: #3A4661;
                width: fit-content;
                font-size: 1.3em;
                padding: 3px 7px;
                position: absolute;
                bottom: 0;
                left: 25%;
            }
        }
        .subCharacter{
            position: absolute;
            animation-duration: 0.4s;
            .character0{
                width: 100%;
                position: relative;
                top: -20px;
                left: 380px;
            }
            .character1{
                width: 100%;
                position: absolute;
                top: 110px;
                left: 500px;
            }
            .character2{
                width: 100%;
                position: absolute;
                top: 170px;
                left: 330px;
            }
        }
    }
}


.animated {
  animation-duration: 0.5s;
}

@keyframes mediumfadeInRight {
  from {
    opacity: 0;
    -webkit-transform: translate3d(25px, 0, 0);
    transform: translate3d(25px, 0, 0);
  }

  to {
    opacity: 1;
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
  }
}
.mediumfadeInRight {
  -webkit-animation-name: mediumfadeInRight;
  animation-name: mediumfadeInRight;
}

@keyframes smallfadeInRight {
  from {
    opacity: 0;
    -webkit-transform: translate3d(12px, 0, 0);
    transform: translate3d(12px, 0, 0);
  }

  to {
    opacity: 1;
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
  }
}
.smallfadeInRight {
  -webkit-animation-name: smallfadeInRight;
  animation-name: smallfadeInRight;
}
</style>