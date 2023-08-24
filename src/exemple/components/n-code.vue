<template>
  <div class="code-container">
    <div class="green">
      <span class="grey">{{ `<` }}</span>
      <span>{{ "NTransition" }}</span>
    </div>
    <div v-show="animateProps.appear">&nbsp&nbspappear</div>
    <div>
      <span>&nbsp&nbsp</span>
      <span class="white">:</span>
      <span>show</span>
      <span class="white">="</span>
      <span class="blue">{{ animateProps.show }}</span>
      <span class="white">"</span>
    </div>
    <div v-show="animateStr">
      <span>&nbsp&nbsp</span>
      <span v-show="!anmtIsStr" class="white">:</span>
      <span>animate</span>
      <span class="white">=<span :class="{'red':anmtIsStr}">"</span></span>
      <span v-html="animateStr" :class="{'red':anmtIsStr}"></span>
      <span :class="anmtIsStr ? `white` : `red`">"</span>
    </div>
    <div v-show="durationStr">
      <span>&nbsp&nbsp</span>
      <span class="white">:</span>
      <span>duration</span>
      <span class="white">="</span>
      <span v-html="durationStr"></span>
      <span class="white">"</span>
    </div>
    <div v-show="delayStr">
      <span>&nbsp&nbsp</span>
      <span class="white">:</span>
      <span>delay</span>
      <span class="white">="</span>
      <span class="cyan">{{ delayStr }}</span>
      <span class="white">"</span>
    </div>
    <div class="grey">></div>
    <div>
      <span>&nbsp&nbsp</span>
      <span class="grey">{{ `<` }}</span>
      <span class="blue">div</span>
      <span> class</span>
      <span class="white">=</span>
      <span class="red">"full f-c-c"</span>
      <span class="grey">></span>
    </div>
    <div>
      <span>&nbsp&nbsp&nbsp&nbsp</span>
      <span class="grey">{{ `<` }}</span>
      <span class="blue">div</span>
      <span> class</span>
      <span class="white">=</span>
      <span class="red">"callout-title"</span>
      <span class="grey">></span>
      <span class="white">VueAnimate</span>
      <span class="grey">{{ `</` }}</span>
      <span class="blue">div</span>
      <span class="grey">></span>
    </div>
    <div>
      <span>&nbsp&nbsp</span>
      <span class="grey">{{ `</` }}</span>
      <span class="blue">div</span>
      <span class="grey">></span></div>
    <div>    
      <div class="green">
        <span class="grey">{{ `</` }}</span>
        <span>{{ "NTransition" }}</span>
        <span class="grey">></span>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from "vue";
import type {
  AnimateDuration,
  AnimateTypes,
} from "../../components/n-transition";

interface AnimateProps {
  show: boolean;
  animate: string | AnimateTypes;
  duration: number | AnimateDuration;
  delay: number;
  appear: boolean;
}
const props = defineProps<{ animateProps: AnimateProps }>();

const anmtIsStr = ref(false);

const animateStr = computed(() => {
  let animate = props.animateProps.animate;
  if (typeof animate == "string") {
    anmtIsStr.value = true;
    return animate;
  };
  if (animate.in == animate.out) {
    anmtIsStr.value = true;
    return animate.in;
  };
  anmtIsStr.value = false;
  if (animate.in == "fadeIn" && animate.out == "fadeOut") return false;
  return `<span class="yellow">{</span>`
     + ` in<span class="white">:</span>` 
     + ` <span class="red">'${animate.in}'</span>` 
     + `<span class="white">,</span>` 
     + ` out<span class="white">:</span>` 
     + ` <span class="red">'${animate.out}'</span>` 
     + ` <span class="yellow">}</span>`;
});

const durationStr = computed(() => {
  let duration = props.animateProps.duration;
  if (typeof duration == "number") return duration == 1 ? false : duration;
  if (duration.in == duration.out)
    return duration.in == 1 ? false : duration.in;
  return `<span class="yellow">{</span>`
     + ` in<span class="white">:</span>` 
     + ` <span class="cyan">${duration.in}</span>` 
     + `<span class="white">,</span>` 
     + ` out<span class="white">:</span>` 
     + ` <span class="cyan">${duration.out}</span>` 
     + ` <span class="yellow">}</span>`;
});

const delayStr = computed(() => {
  let delay = props.animateProps.delay;
  return delay == 0 ? false : delay;
});
</script>

<style>
.code-container {
  text-align: left;
  font-family: Consolas, 'Courier New', monospace;
  color: #9cdcfe;
  background-color: #1f1f1f;
  padding: 1em;
  border-radius: 1em;
  margin: 0;
}

.blue {
  color: #549cd6;
}

.white {
  color: #cccccc;
}

.grey {
  color: #808080;
}

.green {
  color: #4ec9b0;
}

.red {
  color: #ce9178;
}

.yellow {
  color: #ffd700;
}

.cyan {
  color: #b5cea8;
}
</style>
