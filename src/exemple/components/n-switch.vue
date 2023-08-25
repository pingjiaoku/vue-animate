<template>
  <div
    class="switch"
    :class="{ 'switch-on': checked }"
    :value="value"
    @click="checked = !checked"
  ></div>
</template>

<script lang="ts" setup>
import { ref, watch } from "vue";

const props = defineProps<{ value: boolean }>();
const emits = defineEmits(["update:value"]);

const checked = ref(props.value);

watch(
  () => checked.value,
  (newValue) => {
    emits("update:value", newValue);
  },
  { immediate: true }
);
</script>

<style scoped>
.switch {
  position: relative;
  width: 44px;
  height: 24px;
  border: 1px solid #dfdfdf;
  border-radius: 12px;
  background-color: #ffffff;
  transition: all 0.3s;
  cursor: pointer;
}
.switch:after {
  content: " ";
  position: absolute;
  top: 0;
  left: 0;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background-color: #ffffff;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.4);
  transition: transform 0.35s;
}
.switch-on {
  border-color: #351c75;
  background-color: #351c75;
}
.switch-on:after {
  transform: translateX(20px);
}
</style>
