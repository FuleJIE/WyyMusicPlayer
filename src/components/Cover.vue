<script setup lang="ts">
import transformNumber from '@/utils/transformNumber'
const props = defineProps<{
  picUrl: string
  playCount?: number
  round?: boolean
  hoverLayer?: boolean
  hoverPlay?: boolean
  skeleton?: string
}>()

const playCountText = computed(() => {
  if (props.playCount) return transformNumber(props.playCount)
  else return 0
})

/* 图片骨架屏 */
const loading = ref(true)
</script>

<template>
  <div
    class="toplist-cover-container"
    :class="{
      round: round,
      'hover-layer': hoverLayer,
      'hover-play': hoverPlay,
    }"
  >
    <!-- 图片骨架屏 -->
    <el-skeleton animated v-if="loading" style="height: 100%">
      <template #template>
        <el-skeleton-item
          variant="image"
          :style="{ height: `${skeleton}` || '100%', width: '100%' }"
        />
      </template>
    </el-skeleton>
    <!-- 图片 -->
    <div class="picture-wrapper" v-show="!loading">
      <img :src="picUrl" alt="" @load="loading = false" @error="" />
    </div>
    <!-- 播放量 -->
    <div class="playCount-container" v-if="playCount">
      <i-carbon-play class="text-3" />
      <div class="play-count" text="3 white">{{ playCountText }}</div>
    </div>
  </div>
</template>

<style lang="less" scoped>
.toplist-cover-container {
  position: relative;
  height: 100%;
  width: 100%;

  .picture-wrapper {
    width: 100%;
    height: 100%;
  }

  &.round {
    overflow: hidden;
    border-radius: 10px;
  }

  .playCount-container {
    display: flex;
    align-items: center;
    position: absolute;
    right: 5px;
    top: 5px;
    color: #fff;
    text-align: middle;
  }
}
</style>
