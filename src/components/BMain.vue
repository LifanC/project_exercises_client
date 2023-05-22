<script setup>

import PubSub from "pubsub-js";
import {ref} from "vue";
const tableData = ref()
const show = ref()

const token =  PubSub.subscribe('BMain', function (msg, data) {
  tableData.value = data
});
// PubSub.unsubscribe(token)

PubSub.subscribe('noGetAll',function (msg, data){
  show.value = data
})

</script>

<template>
  <div v-show="show">
    <el-table :data="tableData" height="500px" style="width: 100%">
      <el-table-column
          prop="currencyName"
          label="幣別名稱"
          width="100px"
      />
      <el-table-column
          prop="currency"
          label="幣別"
          width="100px"
      />
      <el-table-column
          prop="currencyNationArray"
          label="國家"
      />

    </el-table>
  </div>

</template>

<style scoped>

</style>