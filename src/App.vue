<template>
  <div>
    <router-view/>
  </div>
</template>

<script setup>
</script>


<style lang="scss">
$lightfont: #F6F2EE;
$darkfont: #565656;
$grayfont: #B4A08C;


@font-face {
  font-family: "hk4e";
  src: url('./assets/hk4e_zh-cn.ttf');
}

body{
  cursor: url('./assets/ui/cursor.webp'), pointer;
  margin: 0;
  background: #272833;
  font-size: 1rem;
  background: url('@/assets/ui/background.webp');
}
#app {
  font-family: 'hk4e';
  color: $darkfont;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>