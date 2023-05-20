<script lang="ts" setup>

import {Pointer, CaretBottom, Search, Refresh} from "@element-plus/icons-vue";
import PubSub from 'pubsub-js'
import axios from "axios";
import {reactive, ref} from "vue";

function getList() {
  axios.get('http://localhost:8080/getAll')
      .then((res) => {
        tableData.value = res.data
        PubSub.publish('transfer', res.data);
      }).catch((error) => {
    console.log('error', error)
  })
  axios.get('http://localhost:8080/getNation')
      .then((res) => {
        tableDataName.value = res.data
      }).catch((error) => {
    console.log('error', error)
  })

}

getList()

const handleOpen = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}
const handleClose = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}

function rateSubmit() {
  // 發布消息(消息關鍵字, 要傳遞的資料)
  axios.get('https://api.exchangerate-api.com/v4/latest/TWD')
      .then((response) => {
        axios({
          method: 'post',
          url: 'http://localhost:8080/getRate',
          data: response.data
        }).then((res) => {
          if (res.data[0] == "true") {
            ElMessage({
              showClose: true,
              type: "success",
              message: res.data[1],
            })
          } else if (res.data[0] == "false") {
            ElMessage({
              showClose: true,
              type: "error",
              message: res.data[1],
            })
          }
        })
      })
      .catch((error) => {
        console.log('rateSubmit_catch', error)
      })
  setTimeout(() => {
    getList()
  }, 1000)

}

const fromData = reactive({
  v: 0
})

console.log('fromData', fromData.v)
fromData.v++
console.log('fromData', fromData.v)

const rateName = ref('')
const nation = ref('')
const nationName = ref('')
const nationDataName = ref()
const tableData = ref()
const tableDataName = ref()

function inquiry() {
  axios.get('http://localhost:8080/getOnly', {
    params: {
      curField: rateName.value
    }
  })
      .then((res) => {
        PubSub.publish('transfer', res.data);
      })
      .catch((error) => {
        console.log('error', error)
      })
}

function getNationName(){
  axios.get('http://localhost:8080/getNationName', {
    params: {
      curField: rateName.value
    }
  })
      .then((res) => {
        nationDataName.value = res.data
        nationName.value = JSON.parse(nationDataName.value[0].currencyNation)
        nation.value = ''

      })
      .catch((error) => {
        console.log('error', error)
      })
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
          查詢匯率
        </template>
        <el-menu-item index="1-1">
          <el-button
              type="primary" plain
              size="large"
              :icon="Pointer"
              @click="rateSubmit"
          >更新當天匯率
          </el-button>
        </el-menu-item>
        <el-menu-item index="1-2">
          <el-select v-model="rateName" size="large" placeholder="請選擇" filterable>
            <el-option
                v-for="item in tableDataName"
                :key="item.currency"
                :label="item.currency +' '+ item.currencyName"
                :value="item.currency"
                @click="getNationName"
            />
          </el-select>
        </el-menu-item>
        <el-menu-item index="1-2">
          <el-text>查詢幣別</el-text>
          &emsp;
          <el-button
              type="primary"
              :icon="Search"
              @click="inquiry"
          />
          &emsp;
          <el-text>重整</el-text>
          &emsp;
          <el-button
              :icon="Refresh"
              @click="getList"
          />
        </el-menu-item>
        <el-menu-item index="1-3">
          <el-select v-model="nation" size="large" placeholder="使用的國家">
            <el-option
                v-for="item in nationName"
                :key="item"
                :label="item"
                :value="item"
            />
          </el-select>
        </el-menu-item>
      </el-sub-menu>
    </el-menu>
  </el-aside>

</template>

<style scoped>

</style>