<template>
  <div v-if="isMounted" class="root">
    <button @click="refreshPage">刷新组件</button>
    <div class="full" style="flex: 6; width: 1000px; height: 800px">
      <NTransition
        ref="transitionRef"
        appear
        :duration="2"
        :show="showDiv"
        :animate="tranAnimate"
      >
        <div class="full f-c-c">
          <div class="text">AnimateVue</div>
        </div>
      </NTransition>
    </div>
    <div style="flex: 1">
      <button v-for="anmt in animates" @click="startAnimation(anmt)">
        {{ anmt }}
      </button>
    </div>
    <button @click="showDiv = !showDiv">开始动画</button>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import { NTransition } from "./components/n-transition";

const showDiv = ref(false);
const isMounted = ref(true);

const refreshPage = () => {
  isMounted.value = false;
  setTimeout(() => {
    isMounted.value = true;
  }, 100);
};

const tranAnimate = ref("bounce");
const transitionRef = ref();

const startAnimation = (animateItem: string) => {
  tranAnimate.value = animateItem;
  transitionRef.value.startAnimation();
};

const animates = [
  "bounce",
  "flash",
  "pulse",
  "rubberBand",
  "shakeX",
  "shakeY",
  "headShake",
  "swing",
  "tada",
  "wobble",
  "jello",
  "heartBeat",
];

const backIn = ["backInDown", "backInLeft", "backInRight", "backInUp"];
const backOut = ["backOutDown", "backOutLeft", "backOutRight", "backOutUp"];

const bounceIn = [
  "bounceIn",
  "bounceInDown",
  "bounceInLeft",
  "bounceInRight",
  "bounceInUp",
];
const bounceOut = [
  "bounceOut",
  "bounceOutDown",
  "bounceOutLeft",
  "bounceOutRight",
  "bounceOutUp",
];

const fadeIn = [
  "fadeIn",
  "fadeInDown",
  "fadeInDownBig",
  "fadeInLeft",
  "fadeInLeftBig",
  "fadeInRight",
  "fadeInRightBig",
  "fadeInUp",
  "fadeInUpBig",
  "fadeInTopLeft",
  "fadeInTopRight",
  "fadeInBottomLeft",
  "fadeInBottomRight",
];

const fadeOut = [
  "fadeOut",
  "fadeOutDown",
  "fadeOutDownBig",
  "fadeOutLeft",
  "fadeOutLeftBig",
  "fadeOutRight",
  "fadeOutRightBig",
  "fadeOutUp",
  "fadeOutUpBig",
  "fadeOutTopLeft",
  "fadeOutTopRight",
  "fadeOutBottomLeft",
  "fadeOutBottomRight",
];

const flip = ["flip"];
const flipIn = ["flipInX", "flipInY"];
const flipOutX = ["flipOutX", "flipOutY"];

const lightspeedIn = ["lightSpeedInRight", "lightSpeedInLeft"];
const lightSpeedOut = ["lightSpeedOutRight", "lightSpeedOutLeft"];
</script>

<style>
html,
body,
#app,
.root {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}

.root {
  display: flex;
}

.full {
  width: 100%;
  height: 100%;
}

.f-c-c {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.text {
  color: #351c75;
  font-size: 100px;
  font-weight: 700;
}
</style>
