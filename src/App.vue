<script setup>
import AppAside from "@/components/AppAside.vue";
import CurrencyMain from "@/components/CurrencyMain.vue";
import BMain from "@/components/BMain.vue";
import {ref} from "vue";

const show1 = ref()
const show2 = ref()

PubSub.subscribe('main',function (msg, data){
  //[?,?] = [1,true]、[1,false]
  switch (data[0]){
    case '1':
      show1.value = data[1]
      break
    case '2':
      show2.value = data[1]
      break
  }
})

</script>
<template>
  <el-container>
    <el-container>
      <AppAside />
      <el-container>
        <el-header ></el-header>
        <div v-show="show1">
          <CurrencyMain />
        </div>
        <div v-show="show2">
          <BMain />
        </div>

      </el-container>
    </el-container>
  </el-container>
</template>

<style scoped>

</style>

