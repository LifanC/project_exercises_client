<script lang="ts" setup>

import {CaretBottom} from "@element-plus/icons-vue";
import PubSub from 'pubsub-js'
import axios from "axios";
import {ref} from "vue";

const rateName = ref('')
const tableDataName = ref()
//table畫面
const noGetAll = ref(false)

function getList() {
  PubSub.publish('noGetAll', noGetAll.value)
  axios.get('http://localhost:8080/getNation')
      .then((res) => {
        tableDataName.value = res.data
      }).catch((error) => {
    console.log('error', error)
  })
}

getList()

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
          url: 'http://localhost:8080/getRate',
          data: response.data
        })
      })
      .catch((error) => {
        console.log('rateSubmit_catch', error)
      })
  setTimeout(() => {
    getList()
  }, 1000)

}


function inquiry() {
  noGetAll.value = true
  PubSub.publish('noGetAll', noGetAll.value)
  axios.get('http://localhost:8080/getOnly', {
    params: {
      curField: rateName.value
    }
  })
      .then((res) => {
        PubSub.publish('transfer', res.data);
        PubSub.publish('getNationCurMoney', res.data);
        PubSub.publish('getRate', rateName.value);
      })
      .catch((error) => {
        console.log('error', error)
      })
  axios.get('http://localhost:8080/getNationOnly', {
    params: {
      curField: rateName.value
    }
  })
      .then((res) => {
        PubSub.publish('BMain', res.data);
      })
      .catch((error) => {
        console.log('error', error)
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

const chatperOpen = (key) => {
  let y = [true, key]
  PubSub.publish('chatper', y)
}
const chatperClose = (key) => {
  let y = [false, key]
  PubSub.publish('chatper', y)
}


</script>

<template>
  <el-aside width="300px">
    <h1 style="padding-left: 100px">Demo</h1>
    <el-menu
        @open="handleOpen"
        @close="handleClose"
    >
      <el-sub-menu index="1">
        <template #title>
          <el-icon>
            <CaretBottom/>
          </el-icon>
          查詢幣別匯率
        </template>
        <el-menu-item index="1-1">
          <el-select v-model="rateName" size="large" placeholder="查詢幣別" filterable>
            <el-option
                v-for="item in tableDataName"
                :key="item.currency"
                :label="item.currency +' '+ item.currencyName"
                :value="item.currency"
                @click="inquiry"
            />
          </el-select>
        </el-menu-item>
      </el-sub-menu>
      <el-sub-menu index="2">
        <template #title>
          <el-icon>
            <CaretBottom/>
          </el-icon>
          U Bike 查詢
        </template>
        <el-menu-item index="2-1">
          <el-text>YouBike2.0臺北市公共自行車即時資訊</el-text>
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
          KUSO英文法
        </template>
        <el-menu
            @open="chatperOpen"
            @close="chatperClose"
        >
          <el-sub-menu index="1">
            <template #title>
              <el-text>Chatper(1)五大句型</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="2">
            <template #title>
              <el-text>Chatper(2)時式</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="3">
            <template #title>
              <el-text>Chatper(3)語態</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="4">
            <template #title>
              <el-text>Chatper(4)助動詞</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="5">
            <template #title>
              <el-text>Chatper(5)名詞子句</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="6">
            <template #title>
              <el-text>Chatper(6)形容詞子句</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="7">
            <template #title>
              <el-text>Chatper(7)連接句</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="8">
            <template #title>
              <el-text>Chatper(8)假設語氣</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="9">
            <template #title>
              <el-text>Chatper(9)不定詞</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="10">
            <template #title>
              <el-text>Chatper(10)動名詞</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="11">
            <template #title>
              <el-text>Chatper(11)分詞</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="12">
            <template #title>
              <el-text>Chatper(12)形容詞與副詞</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="13">
            <template #title>
              <el-text>Chatper(13)代名詞</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="14">
            <template #title>
              <el-text>Chatper(14)倒裝句</el-text>
            </template>
          </el-sub-menu>
          <el-sub-menu index="15">
            <template #title>
              <el-text>Chatper(15)句型大補帖</el-text>
            </template>
          </el-sub-menu>
        </el-menu>
      </el-sub-menu>
    </el-menu>
  </el-aside>

</template>

<style scoped>

</style>
