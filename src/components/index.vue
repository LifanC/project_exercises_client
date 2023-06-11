<script setup>
import {onMounted, onUnmounted, reactive, ref} from "vue";
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

function calendar() {
  const date = calendarValue.value
  const year = date.getFullYear()
  const month = date.getMonth() + 1
  const day = date.getDate()
  return `${year}/${month}/${day}`
}

const fromData = reactive({
  calendarDetails: calendar(),
  ex: ''
})

function handle() {
  visible.value = true
  dialogFormVisible.value = true
  calendarDetails.value = calendar()
  // console.log(triggerRef.value.getBoundingClientRect())
  axios({
    method: 'get',
    url: 'http://localhost:8080/function/get_ins_del',
    params:{
      calendarDetails: calendar()
    }
  })
      .then((response) => {
        if(response.data.length === 0){
          fromData.ex = ''
        }else{
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
      ins: '1'
    }
  })
      .then((response) => {
        fromData.ex = response.data[0].ex
        fromData.ex = ''
        dialogFormVisible.value = false
        ElMessage.success('成功新增')
      })
}

function del() {
  fromData.ex = ''
  axios({
    method: 'post',
    url: 'http://localhost:8080/function/ins_del',
    data: {
      calendarDetails: calendar(),
      ex: fromData.ex,
      del: '1'
    }
  })
      .then((response) => {
        fromData.ex = response.data[0].ex
        fromData.ex = ''
        dialogFormVisible.value = false
        ElMessage('成功清除')
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
                {{ fromData.ex}}
              </template>
            </el-tooltip>
            <el-calendar v-model="calendarValue" @click="handle">
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
    </el-main>
    <el-footer>
      <el-text size="small">Luke版權所有｜ 2023 Luke information Co., Ltd. All Rights Reserved</el-text>
    </el-footer>
  </el-container>

  <el-dialog v-model="dialogFormVisible" :title="calendarDetails" width="500px">
    <el-form :model="fromData">
      <el-form-item label="行程內容">
        <el-input v-model="fromData.ex" type="textarea" style="width: 100%;height: 100%"/>
        <el-button link type="primary" @click="ins">新增</el-button>
        <el-button link type="primary" @click="del">清除</el-button>
      </el-form-item>
    </el-form>
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
</style>