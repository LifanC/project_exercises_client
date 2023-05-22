<script setup>
import AppAside from "@/components/AppAside.vue";
import CurrencyMain from "@/components/CurrencyMain.vue";
import BMain from "@/components/BMain.vue";
import CMain from "@/components/CMain.vue";
import {ref} from "vue";

const show1 = ref()

PubSub.subscribe('main',function (msg, data){
  //[?,?] = [1,true]、[1,false]
  switch (data[0]){
    case '1':
      show1.value = data[1]
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
        <div v-show="show1">
          <BMain />
        </div>
        <div v-show="show1">
          <CMain />
        </div>

      </el-container>
    </el-container>
  </el-container>
</template>

<style scoped>

</style>

