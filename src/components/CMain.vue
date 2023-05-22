<script setup>
import {reactive, ref} from "vue";
import axios from "axios";
import PubSub from "pubsub-js";

const rateName = ref('')
const nation = ref('')
const nationName = ref('')
const nationDataName = ref()
const show = ref(true)
const tableData = ref()

const curFieldMoney = ref(0)

PubSub.subscribe('getNationCurMoney', function (msg, data) {
  curFieldMoney.value = data[0].curFieldMoney
})
PubSub.subscribe('getRate', function (msg, data) {
  rateName.value = data
  getNationName()
})

const fromData = reactive({
  exMoney: 0,
  showMoney: null,
  userName: '',
  userNameQuery: '',
})

function onSubmit() {
  fromData.showMoney = +fromData.exMoney * +curFieldMoney.value
}

function getNationName() {
  axios.get('http://localhost:8080/getNationName', {
    params: {
      curField: rateName.value
    }
  })
      .then((res) => {
        nationDataName.value = res.data
        nationName.value = JSON.parse(nationDataName.value[0].currencyNation)
        nation.value = ''

      })
      .catch((error) => {
        console.log('error', error)
      })
}

// PubSub.subscribe('noGetAll', function (msg, data) {
//   show.value = data
// })

function fromSubmit() {
  if (fromData.exMoney !== 0 && nation.value !== '' && fromData.userName !== '') {
    axios({
      method: 'post',
      url: 'http://localhost:8080/fromSubmit',
      data: {
        userName: fromData.userName,
        nation: nation.value,
        rateName: rateName.value,
        exMoney: fromData.exMoney,
        curFieldMoney: curFieldMoney.value
      }
    }).then((res) => {
      tableData.value = res.data
      fromData.userName = ''
      nation.value = ''
      fromData.exMoney = 0
      fromData.showMoney = null
    })
  } else {
    ElMessage({
      showClose: true,
      message: '請勿空白',
      type: 'error',
    })
  }
}

function getUserMoney() {
  axios.get('http://localhost:8080/getUserMoney', {
    params: {
      userName: fromData.userNameQuery
    }
  }).then((res) => {
    tableData.value = res.data
    if (res.data.length > 0) {
      ElMessage({
        message: '成功',
        type: 'success',
      })
    } else {
      ElMessage.error('重新輸入')
    }
  })
}

function detRow(row) {
  console.log('row',row)
  ElMessageBox.confirm(
      '確定要 delete 嗎?',
      `編號:${row.userNameId}、姓名:${row.userName}`,
      {
        confirmButtonText: '確定',
        cancelButtonText: '取消',
        type: 'warning',
      }
  )
      .then(() => {
        console.log('成功')
        ElMessage({
          type: 'success',
          message: '成功',
        })
      })
      .catch(() => {
        console.log('取消')
        ElMessage({
          type: 'error',
          message: '取消',
        })
      })
}


//Table row的資料
function deitRow(row){
  console.log('row',row)
}


</script>

<template>
  <div v-show="show">
    <el-container>
      <el-aside style="padding-left: 10px" width="250px">
        <el-select v-model="nation" placeholder="查詢國家">
          <el-option
              v-for="item in nationName"
              :key="item"
              :label="item"
              :value="item"
          />
        </el-select>
      </el-aside>
      <el-form :model="fromData">
        <el-row>
          <el-form-item label="請輸入其他國家的金額">
            <el-input
                v-model="fromData.exMoney"
                @input="onSubmit"
            />
          </el-form-item>
          &emsp;
          <el-form-item label="新台幣">
            <el-input
                v-model="fromData.showMoney"
                placeholder="需要多少新台幣"
                disabled
            />
          </el-form-item>
          <!--        </el-row>-->
          &emsp;
          <!--        <el-row>-->
          <el-form-item label="請輸入姓名">
            <el-input v-model="fromData.userName"/>
          </el-form-item>
          &emsp;
          <el-form-item>
            <el-button type="primary" @click="fromSubmit">確定送出</el-button>
          </el-form-item>
        </el-row>
        <el-row>
          <el-form-item>
            <el-text>查詢</el-text>
            <el-input
                v-model="fromData.userNameQuery"
                placeholder="請輸入姓名"
                @blur="getUserMoney"
            />
          </el-form-item>
        </el-row>
      </el-form>
    </el-container>

    <el-table
        :data="tableData"
        style="width: 100%">

      <el-table-column
          prop="userId"
          label="流水編號"
      />
      <el-table-column
          prop="userNameId"
          label="客戶編號"
      />
      <el-table-column
          prop="userName"
          label="客戶名稱"
      />
      <el-table-column
          prop="exMoney"
          label="金額"
      />
      <el-table-column
          prop="rateName"
          label="幣別名稱"
      />
      <el-table-column
          prop="nation"
          label="國家"
      />
      <el-table-column
          prop="curFieldMoney"
          label="匯率"
      />
      <el-table-column
          prop="showMoney"
          label="新台幣"
      />
      <el-table-column>
        <template #default="scope">
          <el-button
              link type="primary"
              @click.prevent="deitRow(scope.row)"
          >Edit</el-button>
          <el-button
              link type="primary"
              @click.prevent="detRow(scope.row)"
          >Delete</el-button>
        </template>
      </el-table-column>
    </el-table>


  </div>


</template>

<style scoped>

</style>