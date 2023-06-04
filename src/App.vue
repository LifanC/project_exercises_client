<script setup>
import AppAside from "@/components/AppAside.vue";
import CurrencyMain from "@/components/CurrencyMain.vue";
import BMain from "@/components/BMain.vue";
import CMain from "@/components/CMain.vue";
import UBikeA from "@/components/UBikeA.vue";
import ToeicA from "@/components/ToeicA.vue";
import MusicA from "@/components/MusicA.vue";
import GodA from "@/components/GodA.vue";

import Chapter1 from "@/components/toeic/Chapter1.vue";
import Chapter2 from "@/components/toeic/Chatper2.vue";

import {ref} from "vue";

const show1 = ref()
const show2 = ref()
const show3 = ref()
const show4 = ref()
const show5 = ref()

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
    case '5':
      show5.value = data[1]
      break
  }
})
PubSub.subscribe('go', function (msg, data) {
  show4.value = data[0]
})

const chapter1 = ref()
const chapter2 = ref()
const chapter3 = ref()
const chapter4 = ref()
const chapter5 = ref()
const chapter6 = ref()
const chapter7 = ref()
const chapter8 = ref()
const chapter9 = ref()
const chapter10 = ref()
const chapter11 = ref()
const chapter12 = ref()
const chapter13 = ref()
const chapter14 = ref()
const chapter15 = ref()
4

PubSub.subscribe('chatper', function (msg, data) {
  //data[0] true:false
  //data[1] key
  if (data[0]) {
    switch (data[1]) {
      case '1':
        chapter1.value = data[0]
        break
      case '2':
        chapter2.value = data[0]
        break
      case '3':
        chapter3.value = data[0]
        break
      case '4':
        chapter4.value = data[0]
        break
      case '5':
        chapter5.value = data[0]
        break
      case '6':
        chapter6.value = data[0]
        break
      case '7':
        chapter7.value = data[0]
        break
      case '8':
        chapter8.value = data[0]
        break
      case '9':
        chapter9.value = data[0]
        break
      case '10':
        chapter10.value = data[0]
        break
      case '11':
        chapter11.value = data[0]
        break
      case '12':
        chapter12.value = data[0]
        break
      case '13':
        chapter13.value = data[0]
        break
      case '14':
        chapter14.value = data[0]
        break
      case '15':
        chapter15.value = data[0]
        break
    }
  } else {
    switch (data[1]) {
      case '1':
        chapter1.value = data[0]
        break
      case '2':
        chapter2.value = data[0]
        break
      case '3':
        chapter3.value = data[0]
        break
      case '4':
        chapter4.value = data[0]
        break
      case '5':
        chapter5.value = data[0]
        break
      case '6':
        chapter6.value = data[0]
        break
      case '7':
        chapter7.value = data[0]
        break
      case '8':
        chapter8.value = data[0]
        break
      case '9':
        chapter9.value = data[0]
        break
      case '10':
        chapter10.value = data[0]
        break
      case '11':
        chapter11.value = data[0]
        break
      case '12':
        chapter12.value = data[0]
        break
      case '13':
        chapter13.value = data[0]
        break
      case '14':
        chapter14.value = data[0]
        break
      case '15':
        chapter15.value = data[0]
        break
    }
  }
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
      <div v-show="show5">
        <GodA/>
      </div>
      <div v-show="chapter1">
        <Chapter1/>
      </div>
      <div v-show="chapter2">
        <Chapter2/>
      </div>
    </el-container>
  </el-container>
</template>

<style scoped>

</style>

