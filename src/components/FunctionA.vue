<script setup>
import {reactive, ref} from "vue";
import axios from "axios";

const dialogFormVisible1 = ref(false)
const register_and_login = ref('')
const userName = ref('未登入')
const userNameId = ref('')
const false_and_true_1 = ref(false)
const false_and_true_2 = ref(false)

const rateName = ref()
const tableDataName = ref()
const tableData = ref()
const tableDataMoney = ref()
const curFieldMoney = ref()
const userId = ref('')

const show = ref(false)
const showFunction = ref(true)
const dialogVshowFindPassWord = ref(true)
const fPassword = ref('')


const dialogFormVisible = ref(false)
const titleValue = ref('')
//存錢或取錢 deposit or withdraw money
const depositOrWithdrawMoney = ref('')

const fromData = reactive({
  userName: '',
  passWord: '',
  exMoney: 0,
  showMoney: null,
  userPhone: '',
  userNameQuery: '',
  setMoney: null,
  dalShowMoney: null,
  dialogShowMoney: 0,
  userNameId: '',
})


PubSub.subscribe('main', function (msg, data) {
  //['6', true、false]
  switch (data[0]) {
    case '6':
      if (data[1]) {
        sign_out_dialog()
        show.value = false
      }
      break
  }
})

const handleSelect = (key, keyPath) => {

}

/**
 * 進入dialog畫面
 * 註冊
 * */
function register_dialog() {
  dialogVshowFindPassWord.value = false
  dialogFormVisible1.value = true
  fromData.userName = ''
  fromData.passWord = ''
  register_and_login.value = '註冊'
  false_and_true_1.value = false
  false_and_true_2.value = false
}

/**
 * 進入dialog畫面
 * 登入
 * */
function login_dialog() {
  fPassword.value = ''
  dialogVshowFindPassWord.value = true
  dialogFormVisible1.value = true
  fromData.userName = ''
  fromData.passWord = ''
  register_and_login.value = '登入'
  false_and_true_1.value = false
  false_and_true_2.value = false
  if (userName.value !== '未登入' && userName.value !== '登入失敗，請再輸入一次' && userName.value !== '註冊成功') {
    ElMessage.error('請先登出')
    dialogFormVisible1.value = false
  }
}

/**
 * 進入dialog畫面
 * 修改
 * */
function edit_dialog() {
  dialogVshowFindPassWord.value = false
  if (userName.value === '未登入') {
    fromData.userName = ''
    fromData.passWord = ''
    false_and_true_1.value = true
    false_and_true_2.value = true
  }
  dialogFormVisible1.value = true
  fromData.passWord = ''
  false_and_true_1.value = true
  register_and_login.value = '修改'
}

const countries = ref([])

function method() {
  axios({
    method: 'get',
    url: 'http://localhost:8080/function/function_money',
    params: {
      userNameId: userNameId.value
    }
  })
      .then((response) => {
        countries.value = JSON.parse(response.data[0].currency_nation)
        tableDataMoney.value = response.data
        cur_number.value = response.data[0].cur_number
        if (response.data[0].cur_number === '') {
          show.value = true
          showFunction.value = true
        } else {
          show.value = false
          showFunction.value = false
        }
      })
}

/**
 * 註冊、登入
 * */
const cur_number = ref('')

function sure() {
  tableDataMoney.value = []
  switch (register_and_login.value) {
    case '註冊':
      if (fromData.userName !== '' && fromData.passWord !== '') {
        axios({
          method: 'post',
          url: 'http://localhost:8080/function/register',
          data: fromData
        })
            .then((response) => {
              sign_out_dialog()
              fPassword.value = ''
              dialogFormVisible1.value = false
              ElMessage.success('註冊成功')
            })
      } else {
        ElMessage.error('請勿空白')
      }
      break
    case '登入':
      if (fromData.userName !== '' && fromData.passWord !== '') {
        axios({
          method: 'post',
          url: 'http://localhost:8080/function/login',
          data: fromData
        })
            .then((response) => {
              fPassword.value = ''
              userName.value = response.data[0]
              userNameId.value = response.data[1]
              dialogFormVisible1.value = false
              if (response.data[1] !== '') {
                method()
              }
            })
      } else {
        ElMessage.error('請勿空白')
      }
      break
    case
    '修改':
      if (fromData.userName !== '' && fromData.passWord !== '') {
        axios({
          method: 'put',
          url: 'http://localhost:8080/function/edit/'
              + userNameId.value + '/'
              + fromData.passWord
        })
            .then((response) => {
              fPassword.value = ''
              tableDataMoney.value = response.data
              ElMessage.success('修改成功')
              dialogFormVisible1.value = false
            })
      } else {
        if (userName.value !== '未登入') {
          ElMessage.error('請勿空白')
        }
      }
      break
  }

}

/**
 * 登出
 * */
function sign_out_dialog() {
  rateName.value = []
  show.value = false
  dialogVshowFindPassWord.value = true
  fPassword.value = ''
  tableDataMoney.value = []
  tableData.value = []
  axios({
    method: 'get',
    url: 'http://localhost:8080/function/sign_out'
  })
      .then((response) => {
        userName.value = response.data[0]
        userNameId.value = response.data[1]
      })
}

/**
 * 查詢幣別
 * */
getList()

function getList() {
  axios.get('http://localhost:8080/rate/getNation')
      .then((response) => {
        tableDataName.value = response.data
      })
}

/**
 * 查詢金額
 * */
const cField = ref('')
const cFieldMoney = ref('')

function inquiry() {
  axios.get('http://localhost:8080/rate/getOnly', {
    params: {
      curField: rateName.value
    }
  })
      .then((response) => {
        tableData.value = response.data
        cField.value = response.data[0].curField
        cFieldMoney.value = response.data[0].curFieldMoney
      })
  show.value = true
}

/**
 * 存幣別、匯率
 * */
function save_cur() {
  if (cField.value !== '') {
    showFunction.value = false
    axios({
      method: 'put',
      url: 'http://localhost:8080/function/save_cur/' + userNameId.value + '/' + cField.value + '/' + cFieldMoney.value
    })
        .then((response) => {
          method()
          ElMessage.success('成功，存取幣別、匯率')
        })
  }
}

function tableRowData(row) {
  dialogFormVisible.value = true
  fromData.userName = row.userName
  fromData.userNameId = row.userNameId
  rateName.value = row.cur_number
  curFieldMoney.value = row.cur_field_money
  fromData.dialogShowMoney = row.show_money
  fromData.setMoney = null
  userId.value = row.function_money_id
  fromData.exMoney = row.ex_money
  fromData.dalShowMoney = null
}

const placeholder = ref()

/**
 * 取錢
 * */
function withdraw(row) {
  placeholder.value = '取錢'
  method()
  if (cur_number.value === '') {
    show.value = true
  } else {
    show.value = false
  }
  tableRowData(row)
  depositOrWithdrawMoney.value = 'withdrawMoney'
  titleValue.value = '取錢'
}

/**
 * 存錢
 * */
function deposit(row) {
  placeholder.value = '存錢'
  method()
  if (cur_number.value === '') {
    show.value = true
  } else {
    show.value = false
  }
  tableRowData(row)
  depositOrWithdrawMoney.value = 'depositMoney'
  titleValue.value = '存錢'
}

function exit() {
  dialogFormVisible.value = false
}

/**
 * 計算
 * */
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

function depositMoneyORwithdrawMoney() {
  if (fromData.setMoney >= 0) {
    axios.put('http://localhost:8080/function/putAddMoney/'
        + fromData.setMoney + '/'
        + curFieldMoney.value + '/'
        + userNameId.value + '/'
        + depositOrWithdrawMoney.value
    ).then(response => {
      fromData.dalShowMoney = null
      tableDataMoney.value = response.data
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

/**
 * 查詢密碼
 * */
function findPassword() {
  if (fromData.userName !== '') {
    axios({
      method: 'get',
      url: 'http://localhost:8080/function/findPassword',
      params: {
        userName: fromData.userName
      }
    })
        .then((response) => {
          fPassword.value = response.data
        })
  } else {
    fPassword.value = 'UserName 請勿空白'
  }
}

</script>

<template>
  <el-divider/>
  <el-container class="layout-container-demo">
    <el-header/>
    <el-main>
      <el-descriptions
          title="會員管理"
          direction="vertical"
          :column="6"
          border
          style="width: 100%;"
      >
        <el-descriptions-item label="客戶編號"><span style="color: blue">{{ userNameId }}</span></el-descriptions-item>
        <el-descriptions-item label="客戶姓名"><span style="color: blue">{{ userName }}</span></el-descriptions-item>
        <el-descriptions-item label="會員註冊">
          <!--      進入註冊功能-->
          <el-button style="width: 100%" size="large" plain type="primary" @click="register_dialog">註冊</el-button>
        </el-descriptions-item>
        <el-descriptions-item label="會員登入">
          <!--      進入登入功能-->
          <el-button style="width: 100%" size="large" plain type="primary" @click="login_dialog">登入</el-button>
        </el-descriptions-item>
        <el-descriptions-item label="會員登出">
          <!--      進入登出功能-->
          <el-button style="width: 100%" size="large" plain type="primary" @click="sign_out_dialog">登出</el-button>
        </el-descriptions-item>
        <el-descriptions-item label="密碼修改">
          <!--      進入修改功能-->
          <el-button style="width: 100%" size="large" plain type="primary" @click="edit_dialog">修改</el-button>
        </el-descriptions-item>
      </el-descriptions>

      <el-dialog v-model="dialogFormVisible1" :title="register_and_login" width="500px">
        <el-form v-model="fromData">
          <el-row>
            <el-form-item label="UserName">
              <el-input v-model="fromData.userName" clearable :disabled="false_and_true_1"/>
            </el-form-item>
          </el-row>
          <el-form-item label="PassWord">
            <el-row>
              <el-input v-model="fromData.passWord" clearable type="password" show-password
                        :disabled="false_and_true_2"/>
            </el-row>
          </el-form-item>
        </el-form>
        <template #footer>
          <el-row>
            <el-form-item label="忘記密碼" v-show="dialogVshowFindPassWord">
              <el-button type="warning" @click="findPassword">查詢</el-button>
            </el-form-item>
            &emsp;
            <el-form-item>
              <el-text>{{ fPassword }}</el-text>
            </el-form-item>
          </el-row>
          <span>
            <el-button type="primary" @click="sure">確定</el-button>
            <el-button type="primary" @click="dialogFormVisible1 = false">取消</el-button>
          </span>
        </template>
      </el-dialog>
      <br>
      <div v-show="show">
        <el-select v-model="rateName" placeholder="選擇幣別" filterable style="width: 100%">
          <el-option
              v-for="item in tableDataName"
              :key="item.currency"
              :label="'💰 '+ item.currency +' ➠ '+ item.currencyName +' ➡️ 國家 ➤ '+JSON.parse(item.currencyNation)"
              :value="item.currency"
              @click="inquiry"
          />
        </el-select>
        &emsp;
        <el-text>選好幣別 ➠</el-text>
        &emsp;
        <el-button type="primary" @click="save_cur">確定</el-button>
      </div>
      <el-table :data="tableData" height="80px" border style="width: 100%">
        <el-empty/>
        <el-table-column
            prop=""
            label="幣別名稱"
            width="100px"
        >新台幣
        </el-table-column>
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
      <br>
      <el-table :data="tableDataMoney" height="100%" border style="width: 100%">
        <el-empty/>
        <el-table-column
            label="功能"
            width="100%"
        >
          <template #default="scope">
            <el-button
                plain
                type="primary"
                :disabled="showFunction"
                @click.prevent="deposit(scope.row)"
            >存錢
            </el-button>
            <br><br>
            <el-button
                plain
                type="primary"
                :disabled="showFunction"
                @click.prevent="withdraw(scope.row)"
            >取錢
            </el-button>
          </template>
        </el-table-column>
        <el-table-column
            prop="userNameId"
            label="客戶編號"
            width="150px"
        />
        <el-table-column
            prop="userName"
            label="客戶名稱"
            width="125px"
        />
        <el-table-column
            prop="cur_number"
            label="幣別編號"
            width="100%"
        />
        <el-table-column
            prop="currency_name"
            label="幣別名稱"
            width="150px"
        />
        <el-table-column
            prop=""
            label="貨幣使用國家"
        >
          <template #default>
            <table>
              <tr v-for="(country,index) in countries">
                <td>{{ index + 1 }}</td>
                &emsp;
                <td>{{ country }}</td>
              </tr>
            </table>
          </template>
        </el-table-column>
        <el-table-column
            prop="ex_money"
            label="外幣金額"
        />
        <el-table-column
            prop="cur_field_money"
            label="匯率"
            width="100%"
        />
        <el-table-column
            prop="show_money"
            label="兌換完新台幣"
        />
        <el-table-column
            prop="createTime"
            label="新增日期"
        />
        <el-table-column
            prop="update_time"
            label="修改日期"
        />
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
                  :placeholder="placeholder"
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
            <el-button type="primary" @click="exit">取消</el-button>
          </el-row>
        </el-form>
      </el-dialog>
    </el-main>
  </el-container>

</template>

<style scoped>
.layout-container-demo .el-header {
  background-color: black;
}
</style>