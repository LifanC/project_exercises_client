<script setup>
import {h, onMounted, onUnmounted, reactive, ref, watch} from "vue";
import axios from "axios";

const calendarValue = ref(new Date())
const calendarDetails = ref()
const visible = ref(false)
const dialogFormVisible = ref(false)

/**
 * 虚拟触发
 * */
const triggerRef = ref({
  getBoundingClientRect() {
    return position.value
  },
})

const position = ref({
  top: 0,
  left: 0,
  bottom: 0,
  right: 0,
})

const mousemoveHandler = (e) => {
  position.value = DOMRect.fromRect({
    width: 0,
    height: 0,
    x: e.clientX,
    y: e.clientY,
  })
}
onMounted(() => {
  document.addEventListener('mousemove', mousemoveHandler)
})

onUnmounted(() => {
  document.removeEventListener('mousemove', mousemoveHandler)
})

const calendarHeader = ref()
const selectDate = (val) => {
  calendarHeader.value.selectDate(val)
}

function calendar() {
  const date = calendarValue.value
  const year = date.getFullYear()
  const month = date.getMonth() + 1
  const day = date.getDate()
  return `${year}/${month}/${day}`
}

const fromData = reactive({
  calendarDetails: calendar(),
  ex: '',
  expense_and_income_number: 'A',
  inputMoney: 0,
  details: ''
})

function handle() {
  calendarDetails.value = calendar()
  visible.value = true
  dialogFormVisible.value = true
  fromData.inputMoney = 0
  fromData.details = ''
  // console.log(triggerRef.value.getBoundingClientRect())
  axios({
    method: 'get',
    url: 'http://localhost:8080/function/get_ins_del',
    params: {
      calendarDetails: calendar()
    }
  })
      .then((response) => {
        if (response.data.length === 0) {
          fromData.ex = ''
        } else {
          fromData.ex = response.data[0].ex
        }
      })
}


function ins() {
  axios({
    method: 'post',
    url: 'http://localhost:8080/function/ins_del',
    data: {
      calendarDetails: calendar(),
      ex: fromData.ex,
      expense_and_income_number: fromData.expense_and_income_number,
      inputMoney: fromData.inputMoney,
      details: fromData.details,
      insDel: '1'
    }
  })
      .then((response) => {
        if (typeof response.data[0] !== 'undefined') {
          fromData.ex = response.data[0].ex
          fromData.ex = ''
          fromData.expense_and_income_number = 'A'
          fromData.inputMoney = 0
          fromData.details = ''
          dialogFormVisible.value = false
          ElMessage.success('成功新增')
        }
      })
}

function del() {
  fromData.ex = ''
  fromData.expense_and_income_number = 'A'
  fromData.inputMoney = 0
  fromData.details = ''
  axios({
    method: 'post',
    url: 'http://localhost:8080/function/ins_del',
    data: {
      calendarDetails: calendar(),
      ex: fromData.ex,
      insDel: '2'
    }
  })
      .then((response) => {
        if (typeof response.data[0] !== 'undefined') {
          fromData.ex = response.data[0].ex
          dialogFormVisible.value = false
          ElMessage('成功清除')
        }
      })
}

const activeName = ref('1')

const datePicker = ref([])
const defaultDateRange = ref([]);

const tableData = ref([])
const tableDataFindIns_del = ref([])
const dialogFormVisible1 = ref(false)

const totalAmount = ref(0)

watch(defaultDateRange, (newValue) => {
  datePicker.value = newValue;
});

function setDefaultDateRange() {
  const currentDate = new Date()
  const firstDayOfMonth = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1)
  const lastDayOfMonth = new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0)
  defaultDateRange.value = [firstDayOfMonth, lastDayOfMonth]
}

PubSub.subscribe('index', function (msg, data) {
  tableDataFindIns_del.value = []
  tableData.value = []
  activeName.value = '1'
})

const handleClick = (tab, event) => {
  // console.log(tab.paneName)
  switch (tab.paneName) {
    case "1":

      break
    case "2":
      setDefaultDateRange()
      break
    case "3":

      break
  }
}

function DatePickerStart() {
  const date = datePicker.value[0]
  const year = date.getFullYear()
  const month = date.getMonth() + 1
  const day = date.getDate()
  return `${year}/${month}/${day}`
}

function DatePickerEnd() {
  const date = datePicker.value[1]
  const year = date.getFullYear()
  const month = date.getMonth() + 1
  const day = date.getDate()
  return `${year}/${month}/${day}`
}

function total_amount() {
  axios({
    method: 'get',
    url: 'http://localhost:8080/function/total_amount',
    params: {
      DatePickerStart: DatePickerStart(),
      DatePickerEnd: DatePickerEnd()
    }
  })
      .then((response) => {
        totalAmount.value = response.data
      })
}

function findDatePicker() {
  if (datePicker.value.length > 0) {
    axios({
      method: 'get',
      url: 'http://localhost:8080/function/findDatePicker',
      params: {
        DatePickerStart: DatePickerStart(),
        DatePickerEnd: DatePickerEnd()
      }
    })
        .then((response) => {
          tableData.value = response.data
        })
  }
  axios({
    method: 'get',
    url: 'http://localhost:8080/function/findIns_del',
    params: {
      DatePickerStart: DatePickerStart(),
      DatePickerEnd: DatePickerEnd()
    }
  })
      .then((response) => {
        tableDataFindIns_del.value = response.data
      })
  total_amount()
}

const setInputMoneyFrom = reactive({
  calendarDetails: '',
  setInputMoney: 0,
  ins_del_data_id: '',
  inputMoney: 0,
})
const a = ref()
const b = ref()
const c = ref()

function openDialog(row) {
  dialogFormVisible1.value = true
  a.value = row.calendarDetails
  b.value = row.details
  c.value = row.expense_and_income_name
  setInputMoneyFrom.calendarDetails = row.calendarDetails
  setInputMoneyFrom.ins_del_data_id = row.ins_del_data_id
  setInputMoneyFrom.expense_and_income_number = row.expense_and_income_number
  setInputMoneyFrom.inputMoney = row.inputMoney

}

function setTableData() {
  axios({
    method: 'post',
    url: 'http://localhost:8080/function/setTableData',
    data: setInputMoneyFrom

  })
      .then((response) => {
        setInputMoneyFrom.setInputMoney = 0
        dialogFormVisible1.value = false
        if (datePicker.value.length > 0) {
          axios({
            method: 'get',
            url: 'http://localhost:8080/function/findDatePicker1',
            params: {
              DatePickerStart: DatePickerStart(),
              DatePickerEnd: DatePickerEnd()
            }
          })
              .then((response) => {
                tableDataFindIns_del.value = response.data
              })
          axios({
            method: 'get',
            url: 'http://localhost:8080/function/findDatePicker',
            params: {
              DatePickerStart: DatePickerStart(),
              DatePickerEnd: DatePickerEnd()
            }
          })
              .then((response) => {
                tableData.value = response.data
              })
          total_amount()
        }
      })
}

function confirmEvent(row) {
  axios({
    method: 'delete',
    url: 'http://localhost:8080/function/del',
    data: row
  })
      .then((response) => {
        ElNotification.success({
          position: 'top-left',
          message: h('h3', null, [
            h('span', null, '刪除成功 '),
            h('i', {style: 'color: teal'}, '請重新查詢!!'),
          ]),
        })
        findDatePicker()
      })
}

/**
 * Java
 * */
const tableA = ref([])
const tableB = ref([])
const tableC = ref([])
const a_arr = ref([])
const b_arr = ref([])
const fromDataJava = reactive({
  a: '',
  b: '',
})

function A_array() {
  a_arr.value.push(fromDataJava.a)
  tableA.value = a_arr.value
}

function A_clear() {
  a_arr.value = []
  tableA.value = a_arr.value
}

function A() {
  if (a_arr.value.length > 0) {
    ElMessage.success('新增成功')
    fetch('http://localhost:8080/JavaExercise/A', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        a: a_arr.value,
        b: []
      })
    })
        .then((response) => {

        })
  }
}

function A_show() {
  fetch('http://localhost:8080/JavaExercise/A_show', {
    method: 'GET'
  })
      .then((response) => {
        return response.json()
      })
      .then((result) => {
        fromDataJava.a = ''
        fromDataJava.b = ''
        result.sort()
        tableA.value = result
      })
}

function B_array() {
  b_arr.value.push(fromDataJava.b)
  tableB.value = b_arr.value
}

function B_clear() {
  b_arr.value = []
  tableB.value = b_arr.value
}

function B() {
  if (b_arr.value.length > 0) {
    ElMessage.success('新增成功')
    fetch('http://localhost:8080/JavaExercise/B', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        a: [],
        b: b_arr.value
      })
    })
        .then((response) => {

        })
  }
}

function B_show() {
  fetch('http://localhost:8080/JavaExercise/B_show', {
    method: 'GET'
  })
      .then((response) => {
        return response.json()
      })
      .then((result) => {
        fromDataJava.a = ''
        fromDataJava.b = ''
        result.sort()
        tableB.value = result
      })
}

function C() {
  fetch('http://localhost:8080/JavaExercise/C', {
    method: 'GET'
  })
      .then((response) => {
        return response.json()
      })
      .then((result) => {
        if(result.length === 0){
          ElMessage.info('目前沒資料')
        }
        fromDataJava.a = ''
        fromDataJava.b = ''
        result.sort()
        tableC.value = result
      })
}

function D() {
  fetch('http://localhost:8080/JavaExercise/D', {
    method: 'GET'
  })
      .then((response) => {
        fromDataJava.a = ''
        fromDataJava.b = ''
      })
}


</script>

<template>
  <el-container class="layout-container-demo">
    <el-header style="text-align: left; font-size: 12px">
      <div class="toolbar">
        <el-avatar :size="100">
          <img src="@/components/img.png" alt="IMG">
        </el-avatar>
        &emsp;
        <el-text style="color: white"><p>Luke Chen's 行程表</p></el-text>
      </div>
    </el-header>
    <el-main>
      <el-tabs v-model="activeName" @tab-click="handleClick">
        <el-tab-pane label="行事曆" name="1">
          <el-row :gutter="10">
            <el-col :span="100">
              <div class="demo-calendar">
                <el-tooltip
                    v-model:visible="visible"
                    effect="light"
                    trigger="hover"
                    virtual-triggering
                    :virtual-ref="triggerRef"
                >
                  <template #content>
                    {{ calendarDetails }}
                    行程內容
                    <hr>
                    {{ fromData.ex }}
                  </template>
                </el-tooltip>
                <el-calendar v-model="calendarValue"
                             @click="handle"
                             ref="calendarHeader"
                >
                  <template #header="{ date }">
                    <span>{{ date }}</span>
                    <el-button-group>
                      <el-button size="small" @click="selectDate('prev-month')">
                        上個月 : 1號
                      </el-button>
                      <el-button size="small" @click="selectDate('today')">
                        今天
                      </el-button>
                      <el-button size="small" @click="selectDate('next-month')">
                        下個月 : 1號
                      </el-button>
                    </el-button-group>
                  </template>
                  <template #date-cell="{ data }">
                    <el-text :class="data.isSelected ? 'is-selected' : ''">
                      {{ data.day.split('-').slice(1).join('/') }}
                      {{ data.isSelected ? '✓' : '' }}
                      <br>
                      {{ data.isSelected ? fromData.ex : '' }}
                    </el-text>
                  </template>
                </el-calendar>
              </div>
            </el-col>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="行事曆管理" name="2">
          <el-row>
            <el-form-item>
              <el-date-picker
                  v-model="datePicker"
                  type="daterange"
                  range-separator="~"
                  start-placeholder="Start date"
                  end-placeholder="End date"
              />
            </el-form-item>
            &emsp;
            <el-form-item>
              <el-button plain type="primary" @click="findDatePicker">查詢</el-button>
            </el-form-item>
            &emsp;
            <el-form-item>
              <el-text size="large" type="warning">總金額 ➠ {{ totalAmount }} ＄</el-text>
            </el-form-item>
          </el-row>
          <el-row>
            <el-table :data="tableDataFindIns_del" height="100%" border style="width: 50%">
              <el-table-column
                  prop="calendarDetails"
                  label="日期"
                  width="150px"
              />
              <el-table-column
                  prop="expense"
                  label="支出"
                  width="150px"
              />
              <el-table-column
                  prop="income"
                  label="收入"
                  width="150px"
              />
              <el-table-column
                  prop="totleMoney"
                  label="金額"
                  width="150px"
              />
              <el-table-column
                  prop="ex"
                  label="備註內容"
              />
            </el-table>
          </el-row>
          <el-divider/>
          <el-row>
            <el-table :data="tableData" height="500px" border style="width: 50%">
              <el-table-column
                  label="功能"
                  width="150px"
              >
                <template #default="scope">
                  <el-button
                      link
                      type="primary"
                      @click.prevent="openDialog(scope.row)"
                  >修改金額
                  </el-button>
                  <el-popconfirm
                      width="220"
                      confirm-button-text="確定"
                      cancel-button-text="取消"
                      title="確定要刪除嗎?"
                      @confirm="confirmEvent(scope.row)"
                  >
                    <template #reference>
                      <el-button link type="primary">刪除資料</el-button>
                    </template>
                  </el-popconfirm>
                </template>
              </el-table-column>
              <el-table-column
                  prop="calendarDetails"
                  label="日期"
                  width="150px"
              />
              <el-table-column
                  prop="expense_and_income_name"
                  label="類型"
                  width="150px"
              />
              <el-table-column
                  prop="inputMoney"
                  label="金額"
                  width="150px"
              />
              <el-table-column
                  prop="details"
                  label="內容"
              />
            </el-table>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="Java多執行緒、Redis" name="3">
          <el-container>
            <el-form v-model="fromDataJava">
              <el-descriptions
                  direction="vertical"
                  :column="4"
                  border
                  style="width: 600px;"
              >
                <el-descriptions-item label="A區 : 暫存資料">
                  <el-input v-model="fromDataJava.a"/>
                  <br>
                  <el-button style="width: 100%;height: 50px" plain type="primary" @click="A_array">儲存暫時資料</el-button>
                  <br>
                  <el-button style="width: 100%;height: 50px" plain type="danger" @click="A_clear">清除暫時資料</el-button>
                  <br>
                  <el-button style="width: 100%;height: 50px" plain type="default" @click="A">確定送出資料</el-button>
                  <br>
                  <el-button style="width: 100%;height: 50px" plain type="warning" @click="A_show">
                    顯示確定送出的資料
                  </el-button>
                </el-descriptions-item>
                <el-descriptions-item label="B區 : 暫存資料">
                  <el-input v-model="fromDataJava.b"/>
                  <br>
                  <el-button style="width: 100%;height: 50px" plain type="primary" @click="B_array">儲存暫時資料</el-button>
                  <br>
                  <el-button style="width: 100%;height: 50px" plain type="danger" @click="B_clear">清除暫時資料</el-button>
                  <br>
                  <el-button style="width: 100%;height: 50px" plain type="default" @click="B">確定送出資料</el-button>
                  <br>
                  <el-button style="width: 100%;height: 50px" plain type="warning" @click="B_show">
                    顯示確定送出的資料
                  </el-button>
                </el-descriptions-item>
                <el-descriptions-item label="功能">
                  <el-button style="width: 100%;height: 200px" link type="success" @click="C">全部資料</el-button>
                </el-descriptions-item>
                <el-descriptions-item label="刪除">
                  <el-button style="width: 100%;height: 200px" link type="danger" @click="D">刪除全部資料</el-button>
                </el-descriptions-item>
              </el-descriptions>
            </el-form>
          </el-container>
          <el-descriptions
              direction="vertical"
              :column="3"
              border
              style="width: 600px;height: 500px;"
          >
            <el-descriptions-item label="(1)">
              <div class="custom-width">
                <el-table :data="tableA" border style="height: 450px;">
                  <el-table-column label="A暫存資料">
                    <template #default="scope">
                      {{ scope.row }}
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </el-descriptions-item>
            <el-descriptions-item label="(2)">
              <div class="custom-width">
                <el-table :data="tableB" border style="height: 450px;">
                  <el-table-column label="B暫存資料">
                    <template #default="scope">
                      {{ scope.row }}
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </el-descriptions-item>
            <el-descriptions-item label="(3)">
              <div class="custom-width">
                <el-table :data="tableC" border style="height: 450px;">
                  <el-table-column label="全部資料">
                    <template #default="scope">
                      {{ scope.row }}
                    </template>
                  </el-table-column>
                </el-table>
              </div>
            </el-descriptions-item>
          </el-descriptions>
        </el-tab-pane>
        <el-tab-pane label="功能4" name="4">D</el-tab-pane>
        <el-tab-pane label="功能5" name="5">E</el-tab-pane>
      </el-tabs>
    </el-main>
    <el-footer>
      <el-text size="small">Luke版權所有｜ 2023 Luke information Co., Ltd. All Rights Reserved</el-text>
    </el-footer>
  </el-container>

  <el-dialog v-model="dialogFormVisible" :title="calendarDetails" width="500px">
    <el-form :model="fromData">
      <el-form-item label="選擇">
        <el-radio-group v-model="fromData.expense_and_income_number">
          <el-radio label="A" border>支出</el-radio>
          <el-radio label="B" border>收入</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="金額">
        <el-input
            v-model.number="fromData.inputMoney"
        />
      </el-form-item>
      <el-form-item label="支出、收入內容">
        <el-input v-model="fromData.details" type="textarea" style="width: 100%;height: 100%"/>
      </el-form-item>
      <el-form-item label="當天備註">
        <el-input v-model="fromData.ex" type="textarea" style="width: 100%;height: 100%"/>
        <el-button link type="primary" @click="ins">新增</el-button>
        <el-button link type="primary" @click="del">清除</el-button>
      </el-form-item>
    </el-form>
  </el-dialog>

  <el-dialog v-model="dialogFormVisible1" title="修改金額" width="500px">
    <el-form :model="setInputMoneyFrom">
      <el-text size="large" type="danger"><h1>日期 : {{ a }}</h1></el-text>
      <el-text size="large"><p>類型 : {{ c }}<br>內容 : {{ b }}</p></el-text>
      <br>
      <el-row>
        <el-form-item>
          <el-input
              v-model.number="setInputMoneyFrom.setInputMoney"
              clearable
          />
        </el-form-item>
      </el-row>
    </el-form>
    <template #footer>
        <span>
          <el-button type="primary" @click="setTableData">確定</el-button>
          <el-button type="primary" @click="dialogFormVisible1 = false">取消</el-button>
        </span>
    </template>
  </el-dialog>

</template>

<style scoped>
.demo-calendar {
  height: 100%;
  width: 700px;
}

.layout-container-demo .el-header {
  background-color: black;
}

.layout-container-demo .toolbar {
  display: inline-flex;
  align-items: center;
  height: 100%;
  right: 20px;
}

.custom-width {
  width: 250px;
}
</style>