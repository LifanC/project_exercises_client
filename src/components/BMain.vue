<script setup>

import PubSub from "pubsub-js";
import {ref} from "vue";
const tableData = ref()
const show = ref(true)

const token =  PubSub.subscribe('BMain', function (msg, data) {
  tableData.value = data
});
// PubSub.unsubscribe(token)

// PubSub.subscribe('noGetAll',function (msg, data){
//   show.value = data
// })

</script>

<template>
  <div v-show="show">
    <el-table :data="tableData" height="80px" border style="width: 100%">
      <el-empty />
      <el-table-column
          prop="currencyNationArray"
          label="國家"
      />

    </el-table>
  </div>

</template>

<style scoped>

</style>