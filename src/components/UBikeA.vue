<script setup>
import {reactive, ref} from "vue";
import axios from "axios";

const show = ref(true)
const tableData = ref()

PubSub.subscribe('main', function (msg, data) {
  switch (data[0]) {
    case '2':
      if (data[1]) {
        uBikeSubmit()
        getOnlyList()
      } else if (!data[1]) {
        return show.value = false
      }
      break
  }
})

const fromData = reactive({
  sarea: '大安區',
})

const sareas = ref([])
const sareasArr = ref([])


function uBikeSubmit() {
  axios.get('https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json')
      .then((response) => {
        axios({
          //先刪除
          method: 'get',
          url: 'http://localhost:8080/delUbike',
        }).then(() => {
          for (let i = 0; i < response.data.length; i++) {
            axios({
              method: 'post',
              url: 'http://localhost:8080/getUbike',
              data: response.data[i]
            })
            sareas.value.push(response.data[i].sarea)
          }
          sareasArr.value = [...new Set(sareas.value)]
        })
      })
      .catch((error) => {
        console.log('uBikeSubmit_catch', error)
      })
}

function getOnlyList() {
  axios.get('http://localhost:8080/getOnlyList', {
    params: {
      sarea: fromData.sarea
    }
  })
      .then((res) => {
        tableData.value = res.data
      })
}

</script>

<template>
  <div v-show="show">
    <el-text>查詢資訊</el-text>
    <el-select v-model="fromData.sarea" placeholder="請輸入地區" filterable>
      <el-option
          v-for="item in sareasArr"
          :key="item"
          :label="item"
          :value="item"
          @click="getOnlyList"
      />
    </el-select>
    <el-divider/>
    <el-table :data="tableData">
      <el-table-column
          prop="sno"
          label="編號"
      />
      <el-table-column
          prop="sna"
          label="名稱"
      />
      <el-table-column
          prop="tot"
          label="總停車格"
          width="100px"
      />
      <el-table-column
          prop="sbi"
          label="可借數"
          width="100px"
      />
      <el-table-column
          prop="sarea"
          label="地區"
          width="100px"
      />
      <el-table-column
          prop="srcUpdateTime"
          label="更新時間"
      />
      <el-table-column
          prop="ar"
          label="地址"
      />

    </el-table>
  </div>

</template>

<style scoped>

</style>