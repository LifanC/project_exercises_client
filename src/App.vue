<script setup>
import AppAside from "@/components/AppAside.vue";
import UBikeA from "@/components/UBikeA.vue";
import ToeicA from "@/components/ToeicA.vue";
import MusicA from "@/components/MusicA.vue";
import GodA from "@/components/GodA.vue";
import FunctionA from "@/components/FunctionA.vue";
import Java from "@/components/Java.vue";
import index from "@/components/index.vue";

import {ref} from "vue";

const show2 = ref()
const show3 = ref()
const show4 = ref()
const show5 = ref()
const show6 = ref()
const show7 = ref()
const showIndex = ref(true)

PubSub.subscribe('main', function (msg, data) {
  showIndex.value = false
  switch (data[0]) {
    case '2':
      show2.value = data[1]
      break
    case '3':
      show3.value = data[1]
      break
    case '5':
      show5.value = data[1]
      break
    case '6':
      show6.value = data[1]
      break
    case '7':
      show7.value = data[1]
      break
  }
})
PubSub.subscribe('go', function (msg, data) {
  showIndex.value = false
  show4.value = data[0]
})

PubSub.subscribe('index', function (msg, data) {
  showIndex.value=true;
})

</script>
<template>
  <el-container>
    <AppAside/>
    <el-container>
      <el-header></el-header>
      <div v-show="showIndex">
        <index/>
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
      <div v-show="show5">
        <GodA/>
      </div>
      <div v-show="show6">
        <FunctionA/>
      </div>
      <div v-show="show7">
        <Java/>
      </div>
    </el-container>
  </el-container>
</template>

<style scoped>

</style>

