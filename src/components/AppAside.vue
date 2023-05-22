<script lang="ts" setup>

import {Pointer, CaretBottom, Refresh} from "@element-plus/icons-vue";
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
  console.log(key, keyPath)
  const x = [key, true]
  PubSub.publish('main', x)
}
const handleClose = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
  const x = [key, false]
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
    </el-menu>
  </el-aside>

</template>

<style scoped>

</style>