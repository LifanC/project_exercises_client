<script lang="ts" setup>

import {Pointer} from "@element-plus/icons-vue";
import axios from "axios";


const handleOpen = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}
const handleClose = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}

const rateSubmit = () => {
  axios.get('https://api.exchangerate-api.com/v4/latest/TWD')
      .then((response) => {
        axios({
            method: 'post',
            url: 'http://localhost:8080/getRate',
            data: response.data
        }).then((res)=>{
          if(res.data[0] == "true"){
            ElMessage({
              showClose: true,
              message: res.data[1],
            })
          }else if(res.data[0] == "false"){
            ElMessage({
              showClose: true,
              message: res.data[1],
            })
          }
        })
        console.log('rateSubmit_then',response)
      })
      .catch((error) => {
        console.log('rateSubmit_catch',error)
      })

}

</script>

<template>
  <el-aside>
    <h1 style="padding-left: 100px">Demo</h1>
    <el-menu
        @open="handleOpen"
        @close="handleClose"
    >
      <el-sub-menu index="1">
        <template #title>
          <span>抓取當天匯率</span>
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
      </el-sub-menu>
    </el-menu>
  </el-aside>

</template>

<style scoped>

</style>