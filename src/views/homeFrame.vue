<template>
  <div class="container">
    <div class="option-box">
      <el-button type="primary" size="default" @click="play">{{
        timer ? '停止' : '自动'
      }}</el-button>
      <el-button
        type="primary"
        size="default"
        v-for="item in presets"
        :key="item.ratio"
        @click="usePreset(item.ratio)"
        >{{ item.name }}</el-button
      >
    </div>
    <!-- <div class="screen-frequency">
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
    </div> -->
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
import { ElMessage } from 'element-plus';
import { ref } from 'vue';

const frequency = ref<number>(0);

const screenFrequency = ref<number>(60);

let frameCount = 0;
let startTime = performance.now();

function refreshRate() {
  frameCount++;
  let currentTime = performance.now();
  if (currentTime - startTime >= 1000) {
    let fps = frameCount / ((currentTime - startTime) / 1000);
    screenFrequency.value = Math.round(fps);
    console.log('刷新率：' + fps.toFixed(2) + '帧/秒');
    frameCount = 0;
    startTime = currentTime;
  }
  requestAnimationFrame(refreshRate);
}

requestAnimationFrame(refreshRate);

// const getUserFrequency = () => {
//   const inputFrequency = prompt('请输入显示器频率');
//   try {
//     const inputParsed = parseFloat(inputFrequency as string);
//     if (inputParsed > 0) {
//       screenFrequency.value = inputParsed;
//     } else {
//       throw new Error('无效');
//     }
//   } catch (err) {
//     alert('无效，请重新输入');
//     getUserFrequency();
//   }
// };
// getUserFrequency();
let timer = ref<unknown>();
const clearTimer = () => {
  clearInterval(timer.value as number);
  timer.value = undefined;
};
const play = () => {
  if (timer.value) {
    clearTimer();
    return;
  }
  timer.value = setInterval(() => {
    frequency.value += screenFrequency.value / 5000;
    if (frequency.value >= screenFrequency.value) {
      clearTimer();
    }
  }, 1000 / 60);
};

const usePreset = (ratio: number) => {
  ElMessage.info('如果感觉不像，或不稳定，请多点几次');
  clearTimer();
  frequency.value = screenFrequency.value / ratio;
};

const presets: Array<{ ratio: number; name: string }> = [
  {
    ratio: 8,
    name: '四象生八卦',
  },
  {
    ratio: 7,
    name: '阴中有阳，阳中有阴',
  },
  {
    ratio: 6,
    name: '六道轮回',
  },
  {
    ratio: 5,
    name: '五行相生相克',
  },
  {
    ratio: 4,
    name: '两仪生四象',
  },
  {
    ratio: 3,
    name: '三生万物',
  },
  {
    ratio: 2,
    name: '阴阳相容',
  },
  {
    ratio: 1,
    name: '起点亦是终点',
  },
];
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
  .option-box {
    position: absolute;
    width: 30%;
    top: 5%;
    left: 5%;
    .el-button {
      margin: 0 5px 5px 0;
    }
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
