<template>
  <div class="button">
    <img :src="require('@/assets/characters/buttons/5-'+Limits.limits[type].star5.split('-')[1]+'.png')"
    alt="" class="character"
    :class="isSwitch === number ? 'characterActive':''">
  </div>
</template>

<script>
export default{
    name: 'HeaderButton',
    props: {
      type: String,
      number: Number,
      isSwitch: Number
    }
}
</script>

<script setup>
import Limits from '../limits'
</script>

<style lang="scss" scoped>
.button{
  transition: 0.1s all;
  position: relative;
  background: url('../assets/ui/buttons/button_unactive.png') no-repeat;
  max-width: 174px;
  max-height: 81px;
  margin: 0 15px;
  margin-bottom: 20px;
  &.router-link-exact-active {
    filter: brightness(1.1);
    color: red;
  }
  &:hover{
    transform: scale(1.1);
  }
  &::after{
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    height: 100%;
    width: 100%;
    transition: 0.1s all;
    background: url('../assets/ui/buttons/button_unactive_after.png') no-repeat;
  }
  .character{
    transition: 0.2s all;
    width: 100%;
    transition-delay: 0.1s;
    // position: relative;
    // bottom: 7px;
  }
}
.characterActive{
  transform: translateY(-6px);
}
</style>