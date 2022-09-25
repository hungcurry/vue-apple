<script setup>
  import { computed, onMounted, ref , reactive } from 'vue';
  const progress = ref(0);
  const progress2 = ref(0);
  const opacity = ref(0);

  let styleObj = reactive({
    width : computed(()=> `${ progress2.value }%` ),
    top : computed(()=> progress2.value + '%' ),
    opacity : opacity,
  });

  onMounted(() =>{
    const app = document.querySelector('#app');
    function scrollHandler(){

      if(app.scrollTop < window.innerHeight) {
        progress.value = 3;
        progress2.value = 29;
        opacity.value = 0;
      }else if (app.scrollTop > window.innerHeight * 2) {
        progress.value = 1;
        progress2.value = 49;
        opacity.value = 1;
      }else {
        // progress = 3 - (縮放２倍 * 捲軸百分比)
        const percent = (app.scrollTop - window.innerHeight) / window.innerHeight;
        progress.value = 3 - ( 2 * percent);
        progress2.value = 29 + ( 20 * percent);
        opacity.value = 0 + (1 * percent);
        //console.log(progress2.value);
        console.log(styleObj);
      }
    };
    app.addEventListener('scroll' , scrollHandler)
  });

  const fixClass = computed(()=>{
    return progress.value > 1;
  });
</script>

<template>
  <div id="section1" class="section text">
    <p class="text-gradient">俐落薄型新詮釋。</p>
  </div>
  <div id="section2" class="section" :style="{'--scale': progress }" :class="{ sticky: fixClass }">
    <div class="phone">
      <img src="https://i.ibb.co/kcYCncP/display-hw-fgrzs4i9aaum-large-2x.jpg" alt="">
      <img 
      class="picView" 
      src="https://i.ibb.co/Jy1PdKr/display-screen-gao00g1nd3u6-large-2x.jpg" alt=""
      :style="styleObj"
      >
    </div>
  </div>
  <div id="section3" class="section">
    <div class="phone">
      <img src="https://i.ibb.co/kcYCncP/display-hw-fgrzs4i9aaum-large-2x.jpg" alt="">
      <img 
      class="picView" 
      src="https://i.ibb.co/Jy1PdKr/display-screen-gao00g1nd3u6-large-2x.jpg" alt="">
    </div>
  </div>
  <div id="section4" class="section text">
    <p class="text-gradient">相機與音響，再進化。</p>
  </div>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  // outline: 1px solid red;
}
body {
  overflow: hidden;  // 隱藏 瀏覽器滾動條
 // 如果#app高度是固定值 例: 1000px 
 // 這時候 “瀏覽器視窗高度” 又小於1000px,就會產生 雙 捲軸出來
}
html {
  --gradient-start: rgb(25 75 181);
  --gradient-end: rgb(0 157 228);
}
.text {
  display: flex;
  justify-content: center;
  align-items : center;
  font-size: 60px;
  font-weight: 700;
}
#app {
  height: 100vh; 
  overflow: auto; // 讓sticky 有作用
}
.section {
  height: 100%;
  position: relative;
  overflow: hidden; // 隱藏超出的元素或圖片
  background-color: black;
}
#section2 {
  --scale:3;
  z-index: 10;
}
#section3 {
  --scale:1;
}
#section2 , #section3 {
  background-color : white;
}
.text-gradient {
  // background-image: linear-gradient(180deg, #09ACF5, #62DB54);
  background-image: linear-gradient(
    90deg,
    var(--gradient-start),
    var(--gradient-end)
  );
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
  -webkit-text-fill-color: rgb(0 0 0 / 0%);
}
.phone {
  // alex 
  // position: absolute;
  position: relative;
  width: 80%;
  height: 100%;
  margin: auto;
  transform: scale(var(--scale));
  z-index: 1;
}
.phone > * {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50% , -50%);
}
.phone > img {
  width: 60%;
  height: auto;
}
.phone .picView {
  width: 49%;
  top: 49%;
}
.sticky {
  position: sticky !important;
  top: 0;
  left: 0;
}
.static {
  position: static !important;
}
.absolute {
  position: absolute !important;
}
.relative {
  position: relative !important;
}

// 測試顏色
#section4 {
  // background-color: yellow;
}
#section2 {
  // background-color: pink;
}
#section3 {
  // background-color: green;
}
.Purple {
  // background-color: darkviolet;
}
</style>


