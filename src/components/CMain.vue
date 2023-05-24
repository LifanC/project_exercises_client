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
const curNameJson = ref()

const dialogFormVisible = ref(false)
const userId = ref('')
//存錢或取錢 deposit or withdraw money
const depositOrWithdrawMoney = ref('')

PubSub.subscribe('getNationCurMoney', function (msg, data) {
  curNameJson.value = data[0].curNameJson
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
  userPhone: '',
  userNameQuery: '',
  setMoney: null,
  dalShowMoney: null,
  dialogShowMoney: 0,
})

const rules = reactive({
  userName: [{required: true, message: '請輸入姓名', trigger: 'blur'}],
  userPhone: [
    {required: true, message: '請輸入電話', trigger: 'blur'},
    {pattern: /^[0-9]{10}$/, message: '電話號碼格式不對'}
  ],
})


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


const ruleForm = ref(null)

function fromSubmit() {
  if (!ruleForm) return
  ruleForm.value.validate((valid) => {
    if (valid && nation.value !== '') {
      axios({
        method: 'post',
        url: 'http://localhost:8080/fromSubmit',
        data: {
          userName: fromData.userName,
          userPhone: fromData.userPhone,
          nation: nation.value,
          rateName: rateName.value,
          exMoney: fromData.exMoney,
          curFieldMoney: curFieldMoney.value,
          curNameJson: curNameJson.value
        }
      }).then((res) => {
        tableData.value = res.data
        fromData.userName = ''
        fromData.userPhone = ''
        nation.value = ''
        fromData.exMoney = 0
        fromData.showMoney = null
      })
    } else {
      ElMessage({
        message: '請選擇國家',
        grouping: true,
        type: 'error',
      })
      return false
    }
  })
}

function getUserMoney() {
  if (fromData.userNameQuery) {
    axios.get('http://localhost:8080/getUserMoney', {
      params: {
        userName: fromData.userNameQuery
      }
    }).then((res) => {
      tableData.value = res.data
      if (res.data.length > 0) {
        ElMessage({
          message: '成功',
          grouping: true,
          type: 'success',
        })
      } else {
        ElMessage({
          message: '重新輸入',
          grouping: true,
          type: 'error',
        })
      }
    })
  }
}


function detRow(row) {
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
        axios.delete('http://localhost:8080/delId/' + row.userId + '/' + row.userName
        ).then(response => {
          tableData.value = response.data
        }).catch(error => {
          console.log('error:', error);
        });
        ElMessage({
          type: 'success',
          grouping: true,
          message: '成功',
        })
      })
      .catch(() => {
        ElMessage({
          message: '取消',
          grouping: true,
          type: 'error',
        })
      })
}


//Table row的資料

function tableRowData(row) {
  dialogFormVisible.value = true
  fromData.userName = row.userName
  fromData.userNameId = row.userNameId
  rateName.value = row.rateName
  nation.value = row.nation
  curFieldMoney.value = row.curFieldMoney
  fromData.dialogShowMoney = row.showMoney
  fromData.setMoney = null
  userId.value = row.userId
  fromData.exMoney = row.exMoney
  curNameJson.value = row.curNameJson
  fromData.dalShowMoney = null
}

const titleValue = ref('')

function deposit(row) {
  tableRowData(row)
  depositOrWithdrawMoney.value = 'depositMoney'
  titleValue.value = '存錢'
}

function withdraw(row) {
  tableRowData(row)
  depositOrWithdrawMoney.value = 'withdrawMoney'
  titleValue.value = '取錢'
}

function calcMoney() {
  if (fromData.setMoney < 0) {
    ElMessage({
      message: '金額不能小於 0',
      grouping: true,
      type: 'error',
    })
  }
  switch (depositOrWithdrawMoney.value) {
    case 'depositMoney':
      let depositNumber = +fromData.setMoney * +curFieldMoney.value
      let depositToFixed = depositNumber.toFixed(4)
      fromData.dalShowMoney = parseFloat(depositToFixed);
      break
    case 'withdrawMoney':
      if (+fromData.setMoney > +fromData.exMoney) {
        ElMessage({
          message: '取錢不能大於儲存金額',
          grouping: true,
          type: 'error',
        })
      } else {
        let number = +fromData.setMoney * +curFieldMoney.value
        let toFixed = number.toFixed(4)
        fromData.dalShowMoney = parseFloat(toFixed)
      }
      break
  }
}

//popconfirm 確定 存錢 ->depositMoneyORwithdrawMoney

function depositMoneyORwithdrawMoney() {
  if (fromData.setMoney >= 0) {
    axios.put('http://localhost:8080/putAddMoney/'
        + fromData.setMoney + '/'
        + curFieldMoney.value + '/'
        + userId.value + '/'
        + fromData.userName + '/'
        + depositOrWithdrawMoney.value
    ).then(response => {
      fromData.dalShowMoney = null
      tableData.value = response.data
      ElMessage({
        message: '成功',
        grouping: true,
        type: 'success',
      })
      dialogFormVisible.value = false
    }).catch(error => {
      console.log('error:', error);
    });
  } else {
    ElMessage({
      message: '金額不能小於 0',
      grouping: true,
      type: 'error',
    })
  }
}

function confirmEvent() {
  if (fromData.setMoney) {
    switch (depositOrWithdrawMoney.value) {
      case 'depositMoney':
        depositMoneyORwithdrawMoney()
        break
      case 'withdrawMoney':
        if (+fromData.setMoney > +fromData.exMoney) {
          ElMessage({
            message: '取錢不能大於儲存金額',
            grouping: true,
            type: 'error',
          })
        } else {
          depositMoneyORwithdrawMoney()
        }
        break
    }
  } else {
    ElMessage({
      message: '請輸入金額，或取消',
      grouping: true,
      type: 'warning',
    })
  }
}


</script>

<template>
  <div v-show="show">
    <el-container>
      <el-aside style="padding-left: 10px" width="250px">
        <el-select v-model="nation" placeholder="選擇國家">
          <el-option
              v-for="item in nationName"
              :key="item"
              :label="item"
              :value="item"
          />
        </el-select>
      </el-aside>
      <el-form :model="fromData" :rules="rules" ref="ruleForm">
        <el-row>
          <el-form-item label="請輸入姓名" prop="userName" required>
            <el-input v-model="fromData.userName"/>
          </el-form-item>
          &emsp;
          <el-form-item label="請輸入電話" prop="userPhone" required>
            <el-input v-model="fromData.userPhone"/>
          </el-form-item>
          &emsp;
          <el-form-item>
            <el-button type="primary" @click="fromSubmit(ruleForm)">新增資料</el-button>
          </el-form-item>
        </el-row>
        <el-row>
          <el-divider/>
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
      <!--      <el-table-column-->
      <!--          prop="userId"-->
      <!--          label="流水號碼"-->
      <!--      />-->
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
          label="儲存金額"
      />
      <el-table-column
          prop="rateName"
          label="幣別"
      />
      <el-table-column
          prop="curNameJson"
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
          label="儲存金額(新台幣)"
      />
      <el-table-column>
        <template #default="scope">
          <el-button
              link type="primary"
              @click.prevent="withdraw(scope.row)"
          >取錢
          </el-button>
          <el-button
              link type="primary"
              @click.prevent="deposit(scope.row)"
          >存錢
          </el-button>
          <el-button
              link type="primary"
              @click.prevent="detRow(scope.row)"
          >刪除
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog v-model="dialogFormVisible" :title="titleValue">
      <el-form :model="fromData">
        <el-row>
          <el-form-item>
            <el-text>客戶編號</el-text>
            <el-input v-model="fromData.userNameId" disabled/>
          </el-form-item>
          <el-form-item>
            <el-text>客戶姓名</el-text>
            <el-input v-model="fromData.userName" disabled/>
          </el-form-item>
          <el-form-item>
            <el-text>幣別</el-text>
            <el-input v-model="rateName" disabled/>
          </el-form-item>
          <el-form-item>
            <el-text>國家</el-text>
            <el-input v-model="nation" disabled/>
          </el-form-item>
        </el-row>
        <el-row>
          <el-form-item>
            <el-text>幣別名稱</el-text>
            <el-input v-model="curNameJson" disabled/>
          </el-form-item>
        </el-row>
        <el-row>
          <el-form-item>
            <el-text>儲存金額</el-text>
            <el-input v-model="fromData.exMoney" disabled/>
          </el-form-item>
          <el-form-item>
            <el-text>匯率</el-text>
            <el-input v-model="curFieldMoney" disabled/>
          </el-form-item>
          <el-form-item>
            <el-text>兌換完，儲存金額(新台幣)</el-text>
            <el-input v-model="fromData.dialogShowMoney" disabled/>
          </el-form-item>
        </el-row>
        <el-row>
          <el-form-item>
            <el-text>{{ titleValue }}金額</el-text>
            <el-input
                v-model="fromData.setMoney"
                placeholder="儲存金額"
                type="number"
                @input="calcMoney"
            />
          </el-form-item>
          <el-form-item>
            <el-text>兌換完，新台幣(取小數點後面4碼)</el-text>
            <el-input
                v-model="fromData.dalShowMoney"
                disabled
            />
          </el-form-item>
        </el-row>
        <el-row>
          <el-popconfirm
              confirm-button-text="確定"
              cancel-button-text="取消"
              :title="'確定要'+titleValue+'嗎?'"
              @confirm="confirmEvent"
          >
            <template #reference>
              <el-button
                  type="primary"
              >確定
              </el-button>
            </template>
          </el-popconfirm>
        </el-row>
      </el-form>
    </el-dialog>


  </div>


</template>

<style scoped>

</style>