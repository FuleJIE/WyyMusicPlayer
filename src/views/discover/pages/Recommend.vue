<script setup lang="ts">
import {
  getBanner,
  getPlaylist,
  getNewSong,
  getMv,
  getPrivateContent,
} from '@/api/discover'
import transformList from '@/utils/transformList'
import { Ref } from 'vue'

import Panel from '../components/Panel.vue'
import Card from '@/components/Card.vue'
import NetCarousel from '@/components/NetCarousel.vue'

/* 轮播图 */
const banners: Ref<Array<any>> = ref([])
getBanner().then((res: { code: number; banners: Array<any> }) => {
  banners.value = res.banners
})

/* 推荐歌单 */
const playlist: Ref<Array<any>> = ref([])
getPlaylist(12).then((res: any) => {
  playlist.value = res.result
})

const recommendPlaylist = computed(() => {
  return transformList(playlist.value, 2)
})

/* 最新音乐 */
const newitem: Ref<Array<any>> = ref([])
getNewSong(12).then((res: any) => {
  newitem.value = res.result
})

const newsonglist = computed(() => {
  return transformList(newitem.value, 4)
})

/* 推荐MV */
const recommendMv: Ref<Array<any>> = ref([])
getMv().then((res: any) => {
  recommendMv.value = res.result
})

/* 独家放送 */
const privateContent: Ref<Array<any>> = ref([])
getPrivateContent().then((res: any) => {
  privateContent.value = res.result
})
</script>

<template>
  <el-scrollbar>
    <div class="recommend-container">
      <!-- 轮播图 -->
      <NetCarousel
        :banners="banners"
        type="card"
        height="200px"
        :interval="3000"
      />

      <!-- 推荐歌单 -->
      <Panel title="推荐歌单">
        <el-row :gutter="20" v-for="row in recommendPlaylist">
          <el-col :span="4" v-for="item in row" :key="item.id">
            <Card :content="item" :to="`/playlist/${item.id}`" />
          </el-col>
        </el-row>
      </Panel>
      <!-- 最新音乐 -->
      <Panel title="最新音乐">
        <el-row :gutter="20" v-for="group in newsonglist">
          <el-col :span="8" v-for="item in group" :key="item.id">
            <Card :content="item" horizontal hoverLayer />
          </el-col>
        </el-row>
      </Panel>
      <!-- 推荐MV -->
      <Panel title="推荐MV">
        <el-row :gutter="20">
          <el-col :span="6" v-for="item in recommendMv" :key="item.id">
            <Card :content="item" />
          </el-col>
        </el-row>
      </Panel>
      <!-- 独家放送 -->
      <Panel title="独家放送">
        <el-row :gutter="20">
          <el-col :span="8" v-for="item in privateContent" :key="item.id">
            <Card :content="item" />
          </el-col>
        </el-row>
      </Panel>
    </div>
  </el-scrollbar>
</template>

<style lang="less" scoped>
.recommend-container {
  height: 100%;
  width: 100%;

  .el-row {
    width: 100%;
    margin-bottom: 10px;
  }
}
</style>
