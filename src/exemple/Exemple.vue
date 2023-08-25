<template>
  <div class="flex full">
    <div class="full left">
      <div class="animation-area">
        <NTransition
          v-if="isMounted"
          ref="transitionRef"
          :show="transProps.show"
          :appear="transProps.appear"
          :duration="transProps.duration"
          :animate="transProps.animate"
          :delay="transProps.delay"
        >
          <div class="full f-c-c">
            <div class="callout-title">VueAnimate</div>
          </div>
        </NTransition>
      </div>
      <div class="code-area">
        <div>
          <NCode :animateProps="transProps"></NCode>
        </div>
      </div>
    </div>
    <div class="right full">
      <div>
        <div>
          <div class="title">控制</div>
          <button @click="transProps.show = !transProps.show" class="margin">
            {{ transProps.show ? "消失" : "出现" }}
          </button>
          <button @click="refreshPage" class="margin">刷新</button>
        </div>
        <div>
          <div class="title">配置</div>
          <div class="flex align-center">
            <div class="label-1" style="flex: 2" title="初次渲染时启用动画">
              appear
            </div>
            <div class="flex" style="flex: 7">
              <NSwitch v-model:value="transProps.appear"></NSwitch>
              <div>
                <NTransition :show="transProps.appear">
                  <i style="color: grey; font-size: 12px; margin-left: 6px">
                    点击 控制-刷新 查看效果
                  </i>
                </NTransition>
              </div>
            </div>
          </div>
          <div class="flex align-center">
            <div class="label-1" title="动画持续时间，单位：s" style="flex: 2">
              duration
            </div>
            <div class="flex align-center" style="flex: 7">
              <div class="w-full" style="flex: 7">
                <transition name="slide-up" mode="out-in">
                  <input
                    v-if="durationSwitch"
                    v-model="transProps.duration"
                    type="number"
                  />
                  <div v-else class="flex align-center w-full" style="flex: 7">
                    <span class="label-2">in</span>
                    <input
                      v-model="(<AnimateDuration>transProps.duration).in"
                      type="number"
                    />
                    <span class="label-2">out</span>
                    <input
                      v-model="(<AnimateDuration>transProps.duration).out"
                      type="number"
                    />
                  </div>
                </transition>
              </div>
              <div
                style="flex: 1"
                class="switch-arrow f-c-c"
                @click="switchDuration"
              >
                ↕
              </div>
            </div>
          </div>
          <div class="flex align-center">
            <div
              class="label-1"
              style="flex: 2"
              title="动画延迟触发时间，单位：s"
            >
              delay
            </div>
            <input v-model="transProps.delay" type="number" style="flex: 7" />
          </div>
        </div>
      </div>

      <div>
        <div class="title">动画</div>
        <div class="animates scroll-y">
          <div v-for="aniKey in Object.keys(animates)">
            <div class="title">{{ aniKey }}</div>
            <div v-if="!!animates[aniKey].common">
              <NButton
                v-for="aniType in animates[aniKey].common"
                :animateName="aniType"
                @clickBtn="startAnimation(aniType)"
                @setAnimate="setAnimate"
              ></NButton>
            </div>
            <div v-if="!!animates[aniKey].in">
              <div v-for="anmt in animates[aniKey].in?.length">
                <NButton
                  :animateName="animates[aniKey].in![anmt - 1]"
                  @clickBtn="startAnimation(animates[aniKey].in![anmt - 1])"
                  @setAnimate="setAnimate"
                ></NButton>
                <NButton
                  :animateName="animates[aniKey].out![anmt - 1]"
                  @clickBtn="startAnimation(animates[aniKey].out![anmt - 1])"
                  @setAnimate="setAnimate"
                ></NButton>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, nextTick } from "vue";
import { NTransition } from "../components/n-transition";
import type { AnimateDuration, AnimateTypes } from "../components/n-transition";
import NSwitch from "./components/n-switch.vue";
import NButton from "./components/n-button.vue";
import NCode from "./components/n-code.vue";

interface AnimateProps {
  show: boolean;
  animate: string | AnimateTypes;
  duration: number | AnimateDuration;
  delay: number;
  appear: boolean;
}

const isMounted = ref(true);
const transitionRef = ref();
const transProps = ref<AnimateProps>({
  show: true,
  appear: false,
  animate: { in: "fadeIn", out: "fadeOut" },
  duration: 1,
  delay: 0,
});
const durationSwitch = ref(true);

const refreshPage = async () => {
  isMounted.value = false;
  await nextTick();
  isMounted.value = true;
};

const setAnimate = ({ type, name }: { type: string; name: string }) => {
  // @ts-ignore
  transProps.value.animate[type] = name;
};

const startAnimation = (animateItem: string) => {
  let tAnimate = JSON.parse(JSON.stringify(transProps.value.animate));
  transProps.value.animate = { in: animateItem, out: animateItem };
  transitionRef.value.startAnimation();

  let duration = transProps.value.duration;
  let time = typeof duration == "number" ? duration : duration.in;
  setTimeout(() => {
    transProps.value.animate = tAnimate;
  }, time * 1000);
};

const switchDuration = () => {
  if (durationSwitch.value) {
    let duration = transProps.value.duration as number;
    transProps.value.duration = {
      in: duration,
      out: duration,
    };
  } else {
    transProps.value.duration = (<AnimateDuration>transProps.value.duration).in;
  }
  durationSwitch.value = !durationSwitch.value;
};

type stringKey = Record<
  string,
  { common?: string[]; in?: string[]; out?: string[] }
>;
const animates: stringKey = {
  "Attention seekers": {
    common: [
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
    ],
  },
  Back: {
    in: ["backInDown", "backInLeft", "backInRight", "backInUp"],
    out: ["backOutDown", "backOutLeft", "backOutRight", "backOutUp"],
  },
  Bouncing: {
    in: [
      "bounceIn",
      "bounceInDown",
      "bounceInLeft",
      "bounceInRight",
      "bounceInUp",
    ],
    out: [
      "bounceOut",
      "bounceOutDown",
      "bounceOutLeft",
      "bounceOutRight",
      "bounceOutUp",
    ],
  },
  Fading: {
    in: [
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
    ],
    out: [
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
    ],
  },
  Flippers: {
    common: ["flip"],
    in: ["flipInX", "flipInY"],
    out: ["flipOutX", "flipOutY"],
  },
  Lightspeed: {
    in: ["lightSpeedInRight", "lightSpeedInLeft"],
    out: ["lightSpeedOutRight", "lightSpeedOutLeft"],
  },
  Rotating: {
    in: [
      "rotateIn",
      "rotateInDownLeft",
      "rotateInDownRight",
      "rotateInUpLeft",
      "rotateInUpRight",
    ],
    out: [
      "rotateOut",
      "rotateOutDownLeft",
      "rotateOutDownRight",
      "rotateOutUpLeft",
      "rotateOutUpRight",
    ],
  },
  Zooming: {
    in: ["zoomIn", "zoomInDown", "zoomInLeft", "zoomInRight", "zoomInUp"],
    out: ["zoomOut", "zoomOutDown", "zoomOutLeft", "zoomOutRight", "zoomOutUp"],
  },
  Sliding: {
    in: ["slideInDown", "slideInLeft", "slideInRight", "slideInUp"],
    out: ["slideOutDown", "slideOutLeft", "slideOutRight", "slideOutUp"],
  },
  Specials: {
    common: ["hinge", "jackInTheBox", "rollIn", "rollOut"],
  },
};
</script>

<style scoped>
.left {
  flex: 6;
}

.right {
  flex: 2;
  text-align: left;
  border-left: 1px solid #f0f0f0;
  padding: 1em;
}

.title {
  font-size: 1.2rem;
  font-weight: bold;
  margin: 0.9em 0.6em 0.3em 0.6em;
}

.switch-arrow {
  color: blue;
  font-weight: bold;
  width: 2em;
  height: 2em;
  cursor: pointer;
}

.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.25s ease-out;
}

.slide-up-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

.slide-up-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}

.label-1 {
  width: 4em;
  margin: 0.6em;
  font-size: 1.05em;
}

.label-2 {
  margin: 0.3em;
  font-size: 0.95em;
}

.animates {
  background-color: #fafafa;
  border-radius: 1em;
  padding: 0.6em;
  height: calc(97vh - 320px);
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

.animation-area {
  width: 100%;
  height: 400px;
}

.code-area {
  width: 100%;
  display: flex;
  justify-content: center;
}
.code-area > div {
  width: 600px;
}

.callout-title {
  color: #351c75;
  font-size: 70px;
  font-weight: 700;
}
</style>
