<template>
  <div class="container">
    <div class="play">
      <el-button type="primary" size="default" @click="play">演示</el-button>
    </div>
    <div class="screen-frequency">
      <span>你的显示器频率</span>
      <el-input-number
        v-model="screenFrequency"
        size="default"
        label="你的显示器频率"
        :min="1"
        :step="1"
        :controls="false"
      >
      </el-input-number>
    </div>
    <TheGreatUltimate
      :frequency="frequency"
      class="the-great-ultimate"
    ></TheGreatUltimate>
    <div class="regulator">
      <span class="title">调节频率</span>
      <el-slider
        v-model="frequency"
        :min="0"
        :max="screenFrequency"
        :step="0.5"
        :show-input="true"
      >
      </el-slider>
    </div>
  </div>
</template>

<script setup lang="ts">
import TheGreatUltimate from '@/components/theGreatUltimate.vue';
import { ref } from 'vue';

const frequency = ref<number>(1);

const screenFrequency = ref<number>(60);

const getUserFrequency = () => {
  const inputFrequency = prompt('请输入显示器频率');
  try {
    const inputParsed = parseFloat(inputFrequency as string);
    if (inputParsed > 0) {
      screenFrequency.value = inputParsed;
    } else {
      throw new Error('无效');
    }
  } catch (err) {
    alert('无效，请重新输入');
    getUserFrequency();
  }
};
getUserFrequency();
const play = () => {
  frequency.value = 0;
  let timer = setInterval(() => {
    frequency.value += screenFrequency.value / 1000;
    if (frequency.value >= screenFrequency.value) {
      clearInterval(timer);
    }
  }, 50);
};
</script>

<style lang="scss" scoped>
.container {
  position: relative;
  width: 100%;
  height: 100vh;
  min-height: 600px;
  display: flex;
  justify-content: center;
  align-items: center;
  .the-great-ultimate {
    width: 30vw;
    height: 30vw;
  }
  .play {
    position: absolute;
    top: 5%;
    left: 5%;
  }
  .screen-frequency {
    position: absolute;
    top: 5%;
    right: 5%;
  }
  .regulator {
    width: 80%;
    position: absolute;
    bottom: 5%;
    :deep(.el-slider__marks) {
      width: calc(100% - 12px);
    }
  }
}
</style>
