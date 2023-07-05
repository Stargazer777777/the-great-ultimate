<template>
  <div class="the-great-ultimate">
    <div class="white outer-circle">
      <div class="black inner-circle"></div>
    </div>
    <div class="black outer-circle">
      <div class="white inner-circle"></div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { computed } from 'vue';

interface Props {
  frequency?: number;
}

const props = defineProps<Props>();

const cycle = computed(() => {
  const frequency = props.frequency || 0.5;
  return 1 / frequency;
});

const cssCycle = computed(() => {
  return cycle.value + 's';
});
</script>

<style lang="scss" scoped>
@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.white {
  background-color: #fff;
}
.black {
  background-color: #000;
}
.the-great-ultimate {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 500px;
  height: 500px;
  border-radius: 50%;
  background: linear-gradient(to right, #fff 50%, #000 50%);
  box-shadow: 0 0 30px 5px rgba($color: #000000, $alpha: 0.3);
  animation: rotate v-bind(cssCycle) linear infinite;
  .outer-circle {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 50%;
    height: 50%;
    border-radius: 50%;
    .inner-circle {
      width: 33%;
      height: 33%;
      border-radius: 50%;
    }
  }
}
</style>
