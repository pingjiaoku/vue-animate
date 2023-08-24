<template>
  <transition
    :appear="appear"
    :enter-active-class="`animate__${animation.in}`"
    :leave-active-class="`animate__${animation.out}`"
  >
    <div
      v-show="show && showF"
      :style="{
        WebkitAnimationDuration: `${amtDuration}s`,
        animationDuration: `${amtDuration}s`,
        WebkitAnimationDelay: `${delay}s`,
        animationDelay: `${delay}s`,
      }"
      class="animate-container"
    >
      <slot></slot>
    </div>
  </transition>
</template>

<script lang="ts" setup>
import { ref, computed } from "vue";
import type { AnimateDuration, AnimateTypes } from "./index";

interface PropsType {
  show: boolean;
  animate?: string | AnimateTypes;
  duration?: number | AnimateDuration;
  delay?: number;
  appear?: boolean;
}
const props = withDefaults(defineProps<PropsType>(), {
  animate: () => ({ in: "fadeIn", out: "fadeOut" }),
  duration: 0.5,
  delay: 0,
  repeat: 1,
  appear: false,
});

const showF = ref(true);

const animation = computed(() => {
  if (is(props.animate, "Object")) return props.animate as AnimateTypes;
  return {
    in: props.animate as string,
    out: props.animate as string,
  };
});

const amtDuration = computed(() => {
  if (is(props.duration, "Number")) return props.duration as number;
  let amtDur = props.duration as AnimateDuration;
  return props.show ? amtDur.in : amtDur.out;
});

const is = (val: any, type: string) => {
  return Object.prototype.toString.call(val) === `[object ${type}]`;
};

const startAnimation = () => {
  showF.value = false;
  setTimeout(() => {
    console.log("停止");

    showF.value = true;
  }, amtDuration.value / 2 + amtDuration.value * (props.repeat - 1));
};

defineExpose({
  startAnimation,
});
</script>

<style scoped>
@import url(./styles/animate.css);

.animate-container {
  width: 100%;
  height: 100%;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
}
</style>
