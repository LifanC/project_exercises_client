<script setup>

import {ref} from "vue";
import PubSub from "pubsub-js";

const tableData = ref()
const show = ref(true)

const token =  PubSub.subscribe('transfer', function (msg, data) {
  tableData.value = data
  // PubSub.unsubscribe(token)
});

// PubSub.subscribe('noGetAll',function (msg, data){
//   show.value = data
// })


</script>

<template>
  <div v-show="show">
    <el-table :data="tableData" height="80px" border style="width: 100%">
      <el-empty />
      <el-table-column
          prop=""
          label="幣別名稱"
          width="100px"
      >新台幣</el-table-column>
      <el-table-column
          prop="curLocal"
          label="本地幣別"
          width="100px"
      />
      <el-table-column
          prop="curLocalMoney"
          label="本地金額"
          width="100px"
      />
      <el-table-column
          prop="curNameJson"
          label="幣別名稱"
          width="200px"
      />
      <el-table-column
          prop="curField"
          label="外地幣別"
          width="100px"
      />
      <el-table-column
          prop="curFieldMoney"
          label="兌換金額(新台幣)"
      />
      <el-table-column
          prop="date"
          label="更新日期"
      />

    </el-table>
  </div>
</template>

<style scoped>

</style>