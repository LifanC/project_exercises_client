<script setup>
import axios from "axios";
import {reactive, ref} from "vue";

PubSub.subscribe('main', function (msg, data) {
  switch (data[0]) {
    case '5':
      if (data[1]) {
        allGod()
        tableArr()
      } else if (!data[1]) {
        fromData.godsName = ''
        fromData.godsChatper = ''
        tableData.value = ''
        godsChatperArr.value = []
      }
      break
  }
})
const YN = ref(true)
const godLength = ref(0)
const tableData = ref()


const fromData = reactive({
  godsName: '',
  godsChatper: ''
})

const godsName = ref([])
const godsNameArr = ref([])

function allGod() {
  pagination(1)
  axios.get('http://localhost:8080/god/allgod')
      .then((response) => {
        godsName.value = []
        for (let i = 0; i < response.data.length; i++) {
          godsName.value.push(response.data[i].name)
        }
        godsNameArr.value = [...new Set(godsName.value)]
      })
}

const godsChatper = ref([])
const godsChatperArr = ref([])

const valNum = ref()
const v = ref([])


function pagination(val) {
  valNum.value = val
  getGodOnly()
}

function getGodOnly() {
  YN.value = false
  axios.get('http://localhost:8080/god/getGodOnly', {
    params: {
      name: fromData.godsName,
      chatper: fromData.godsChatper
    }
  })
      .then((response) => {
        v.value = []
        for (let i = 0; i < (Math.ceil(+(response.data.length))); i = i + 30) {
          v.value.push(i)
        }
        godLength.value = v.value.length * 10
        if (typeof v.value[valNum.value - 1] === 'number') {
          axios.get('http://localhost:8080/god/getTenGodData', {
            params: {
              name: fromData.godsName,
              chatper: fromData.godsChatper,
              skip: v.value[valNum.value - 1]
            }
          })
              .then((response) => {
                tableData.value = response.data
              })
        }
        godsChatper.value = []
        for (let i = 0; i < response.data.length; i++) {
          godsChatper.value.push(response.data[i].chatper)
        }
        godsChatperArr.value = [...new Set(godsChatper.value)]
      })
  axios.get('http://localhost:8080/god/getGodOnly', {
    params: {
      name: fromData.godsName
    }
  }).then((response) => {
    godsChatper.value = []
    for (let i = 0; i < response.data.length; i++) {
      godsChatper.value.push(response.data[i].chatper)
    }
    godsChatperArr.value = [...new Set(godsChatper.value)]
  })
}

const tableDataArr = ref()
function tableArr() {
  axios.get('http://localhost:8080/god/tableArr')
      .then((response) => {
        tableDataArr.value = response.data
      })
}


</script>

<template>
  <el-divider/>
  &emsp;
  <el-text size="large" tag="b">隨機聖經</el-text>
  <el-table :data="tableDataArr" height="300px" border>
    <el-table-column prop="name" label="聖經名稱" width="200px"/>
    <el-table-column prop="chatper" label="章" width="50px"/>
    <el-table-column prop="section" label="節" width="50px"/>
    <el-table-column prop="detailEn" label="英文" />
    <el-table-column prop="detailCn" label="中文" />
  </el-table>
  &emsp;
  <el-text>查詢資訊</el-text>
  &emsp;
  <el-select v-model="fromData.godsName" placeholder="聖經查詢" filterable>
    <el-option
        v-for="item in godsNameArr"
        :key="item"
        :label="item"
        :value="item"
        @click="getGodOnly"
    />
  </el-select>
  <el-select v-model="fromData.godsChatper" placeholder="章" filterable clearable :disabled="YN">
    <el-option
        v-for="item in godsChatperArr"
        :key="item"
        :label="item"
        :value="item"
        @click="getGodOnly"
    />
  </el-select>

  <el-pagination
      @current-change="pagination"
      layout="prev, pager, next"
      :total="godLength"
  />

  <el-table :data="tableData" height="100%" border>
    <el-table-column
        prop="chatper"
        label="章"
        width="50px"
    />
    <el-table-column
        prop="section"
        label="節"
        width="50px"
    />
    <el-table-column
        prop="detailEn"
        label="英文"
    />
    <el-table-column
        prop="detailCn"
        label="中文"
    />
  </el-table>

</template>

<style scoped>

</style>