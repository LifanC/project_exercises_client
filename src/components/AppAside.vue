<script lang="ts" setup>

import {CaretBottom} from "@element-plus/icons-vue";
import PubSub from 'pubsub-js'
import axios from "axios";

const handleOpen = (key: string, keyPath: string[]) => {
  // console.log(key, keyPath)
  const x = [key, true]
  PubSub.publish('main', x)
}
const handleClose = (key: string, keyPath: string[]) => {
  // console.log(key, keyPath)
  const x = [key, false]
  goMusicMethod(x[0], x[1])
  PubSub.publish('main', x)
}

rateSubmit()

function rateSubmit() {
  axios.get('https://api.exchangerate-api.com/v4/latest/TWD')
      .then((response) => {
        axios({
          method: 'post',
          url: 'http://localhost:8080/rate/getRate',
          data: response.data
        })
      })
}


function goMusicMethod(key, x) {
  const go = [x, 0]
  if (key == 4) {
    PubSub.publish('go', go)
  }
}

function go(v) {
  const go = [true, v]
  PubSub.publish('go', go)
}

function index() {
  PubSub.publish('index')
}

</script>

<template>
  <el-aside width="220px">
    <el-button size="large" link @click="index">
      <h1 style="padding-left: 100px">首頁</h1>
    </el-button>
    <el-menu
        @open="handleOpen"
        @close="handleClose"
    >
      <el-sub-menu index="2">
        <template #title>
          <el-icon>
            <CaretBottom/>
          </el-icon>
          U Bike 查詢
        </template>
        <el-menu-item index="2-1">
          <el-text>YouBike2.0臺北市即時資訊</el-text>
        </el-menu-item>
      </el-sub-menu>
      <el-sub-menu index="3">
        <template #title>
          <el-icon>
            <CaretBottom/>
          </el-icon>
          TOEIC 單字
        </template>
        <el-menu-item index="3-1">
          <el-text>TOEIC 單字練習</el-text>
        </el-menu-item>
      </el-sub-menu>
      <el-sub-menu index="4">
        <template #title>
          <el-icon>
            <CaretBottom/>
          </el-icon>
          政府資料開放平臺
        </template>
        <el-menu-item index="4-1">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(1)"
          >音樂表演資訊
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-2">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(2)"
          >戲劇表演資訊
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-3">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(3)"
          >舞蹈表演資訊
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-4">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(4)"
          >親子活動
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-5">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(5)"
          >獨立音樂
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-6">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(6)"
          >展覽資訊
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-7">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(7)"
          >講座資訊
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-8">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(8)"
          >電影
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-11">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(11)"
          >綜藝活動
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-13">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(13)"
          >競賽活動
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-14">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(14)"
          >徵選活動
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-15">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(15)"
          >其他藝文資訊
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-17">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(17)"
          >演唱會
          </el-button>
        </el-menu-item>
        <el-menu-item index="4-19">
          <el-button
              type="primary"
              plain
              size="large"
              @click="go(19)"
          >文化部研習課程
          </el-button>
        </el-menu-item>
      </el-sub-menu>
      <el-sub-menu index="5">
        <template #title>
          <el-icon>
            <CaretBottom/>
          </el-icon>
          聖經
        </template>
        <el-menu-item index="5-1">
          <el-text>聖經</el-text>
        </el-menu-item>
      </el-sub-menu>
      <el-sub-menu index="6">
        <template #title>
          <el-icon>
            <CaretBottom/>
          </el-icon>
          小型外匯所
        </template>
        <el-menu-item index="6-1">
          <el-text>功能</el-text>
        </el-menu-item>
      </el-sub-menu>
    </el-menu>
  </el-aside>

</template>

<style scoped>

</style>
