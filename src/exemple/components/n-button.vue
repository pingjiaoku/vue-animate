<template>
  <div
    class="n-button"
    @mouseenter="showMenu = true"
    @mouseleave="showMenu = false"
  >
    <button class="margin" @click="emits('clickBtn')">
      {{ animateName }}
    </button>
    <div class="fixed">
      <NTransition :show="showMenu" :duration="0.3">
        <div class="contain">
          <div @click="setAnimate('in')">in⬅</div>
          <div @click="setAnimate('out')">➡out</div>
        </div>
      </NTransition>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import { NTransition } from "../../components/n-transition";
const emits = defineEmits(["clickBtn", "setAnimate"]);
const props = defineProps<{ animateName: string }>();

const showMenu = ref(false);

const setAnimate = (type: string) => {
  emits("setAnimate", { type, name: props.animateName });
  showMenu.value = false;
};
</script>

<style scoped>
.n-button {
  display: inline;
  position: relative;
}

.fixed {
  position: absolute;
  left: 50%;
  top: -40px;
  z-index: 10;
  transform: translate(-50%);
  height: 40px;
}

.contain {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  background-color: #ffffffee;
  border-radius: 5px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.4);
  width: 100px;
  height: 30px;
}

.contain > div {
  height: 100%;
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  cursor: pointer;
}
.contain > div:hover {
  background-color: #f0f0f0;
}
</style>
