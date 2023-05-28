<script setup>
import AppAside from "@/components/AppAside.vue";
import CurrencyMain from "@/components/CurrencyMain.vue";
import BMain from "@/components/BMain.vue";
import CMain from "@/components/CMain.vue";
import UBikeA from "@/components/UBikeA.vue";
import ToeicA from "@/components/ToeicA.vue";
import MusicA from "@/components/MusicA.vue";
import {ref} from "vue";

const show1 = ref()
const show2 = ref()
const show3 = ref()
const show4 = ref()

PubSub.subscribe('main', function (msg, data) {
  switch (data[0]) {
    case '1':
      show1.value = data[1]
      break
    case '2':
      show2.value = data[1]
      break
    case '3':
      show3.value = data[1]
      break
  }
})
PubSub.subscribe('go', function (msg, data) {
  show4.value = data[0]
})

</script>
<template>
  <el-container>
    <AppAside/>
    <el-container>
      <el-header></el-header>
      <div v-show="show1">
        <CurrencyMain/>
      </div>
      <div v-show="show1">
        <BMain/>
      </div>
      <div v-show="show1">
        <CMain/>
      </div>
      <div v-show="show2">
        <UBikeA/>
      </div>
      <div v-show="show3">
        <ToeicA/>
      </div>
      <div v-show="show4">
        <MusicA/>
      </div>
    </el-container>
  </el-container>
</template>

<style scoped>

</style>

