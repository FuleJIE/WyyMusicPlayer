<script setup lang="ts">
import { usePlayer } from '@/store/player'
const player = usePlayer()
const value = ref(0.1)

watch(
  () => value.value,
  (newVal, oldVal) => {
    player.setVolume(newVal)
  }
)

// 点击喇叭
const lastValue = ref(0.1)
const muteToggle = () => {
  if (value.value) {
    lastValue.value = value.value
    value.value = 0
  } else {
    value.value = lastValue.value
  }
}
</script>

<template>
  <div class="net-volume">
    <div class="net-range">
      <NetSlider
        v-model="value"
        vertical
        height="100px"
        :min="0"
        :max="1"
        :step="0.01"
      />
    </div>
    <div class="net-icon" @click="muteToggle">
      <i-carbon-volume-down v-if="value" />
      <i-carbon-volume-mute v-else />
    </div>
  </div>
</template>

<style lang="less" scoped>
.net-volume {
  &:hover .net-range {
    display: flex;
  }
  .net-range {
    display: none;
    position: absolute;
    bottom: 100%;
    left: 50%;
    transform: translate(-50%, -10%);
    width: 20px;
    height: 100px;
    justify-content: center;
    padding: 10px 0;
    background-color: #fff;
    box-shadow: 1px 1px 2px 0 rgba(0, 0, 0, 0.3), inset 1px 2px 0 0 #fff;
    border-radius: 10px;

    &::after {
      content: '';
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translate(-50%, 100%);
      border: 5px solid transparent;
      border-top: 5px solid #fff;
    }
  }
}
</style>
