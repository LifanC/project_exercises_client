<script setup>
import {reactive, ref} from "vue";
import axios from "axios";

const show = ref()
const tableData = ref()

const fromData = reactive({
  english: null,
  chinese: null,
  example: null,
  explain: null,
  toeicId: null,
  dialogEnglish: null,
  dialogChinese: null,
  dialogExample: null,
  dialogExplain: null,
})
const rules = reactive({
  english: [{required: true, message: '請輸入英文單字', trigger: 'blur'}],
  chinese: [{required: true, message: '請輸入中文翻譯', trigger: 'blur'}],
  example: [{required: true, message: '請輸入英文例句', trigger: 'blur'}],
  explain: [{required: true, message: '請輸入中文例句翻譯', trigger: 'blur'}],
})

const dialogRules = reactive({
  dialogEnglish: [{required: true, message: '請輸入英文單字', trigger: 'blur'}],
  dialogChinese: [{required: true, message: '請輸入中文翻譯', trigger: 'blur'}],
})

const ruleForm = ref(null)


PubSub.subscribe('main', function (msg, data) {
  switch (data[0]) {
    case '3':
      if (data[1]) {
        toeicWords()
        return show.value = true
      } else if (!data[1]) {
        return show.value = false
      }
      break
  }
})

function toeicWords() {
  axios.get('http://localhost:8080/toeicWords').then((res) => {
    tableData.value = res.data
  })
}

function fromSubmit() {
  if (!ruleForm) return
  ruleForm.value.validate((valid) => {
    if (valid) {
      axios({
        method: 'post',
        url: 'http://localhost:8080/toeicFromSubmit',
        data: {
          english: fromData.english,
          chinese: fromData.chinese
        }
      }).then((res) => {
        ElMessage({
          message: '成功',
          grouping: true,
          type: 'success',
        })
        fromData.english = null
        fromData.chinese = null
      })
    } else {
      ElMessage({
        message: '請勿空白',
        grouping: true,
        type: 'error',
      })
      return false
    }
  })
}

const dialogFormVisible = ref(false)

function fromSubmitEx() {
  if (!ruleForm) return
  ruleForm.value.validate((valid) => {
    if (valid) {
      axios({
        method: 'put',
        url: 'http://localhost:8080/toeicFromSubmitEx/'
            + fromData.example + '/'
            + fromData.explain
      }).then((res) => {
        tableData.value = res.data
        ElMessage({
          message: '成功',
          grouping: true,
          type: 'success',
        })
        fromData.example = null
        fromData.explain = null
      })
    } else {
      ElMessage({
        message: '請勿空白',
        grouping: true,
        type: 'error',
      })
      return false
    }
  })
}

function editRow(row) {
  dialogFormVisible.value = true
  fromData.toeicId = row.toeicId
  fromData.dialogEnglish = row.english
  fromData.dialogChinese = row.chinese
  fromData.dialogExample = row.example
  fromData.dialogExplain = row.explain
}

function setData() {
  if (!ruleForm) return
  ruleForm.value.validate((valid) => {
    if (valid) {
      axios({
        method: 'put',
        url: 'http://localhost:8080/setData/'
            + fromData.toeicId + '/'
            + fromData.dialogEnglish + '/'
            + fromData.dialogChinese + '/'
            + fromData.dialogExample + '/'
            + fromData.dialogExplain
      }).then((res) => {
        ElMessage({
          message: '成功',
          grouping: true,
          type: 'success',
        })
        tableData.value = res.data
        dialogFormVisible.value = false
      })
    }else{
      ElMessage({
        message: '請勿空白',
        grouping: true,
        type: 'error',
      })
      return false
    }
  })
}


</script>

<template>
  <div v-show="show">
    <el-divider/>
    <el-text>新增單字</el-text>
    <el-row>
      <el-container>
        <el-form :model="fromData" :rules="rules" ref="ruleForm">
          <el-row>
            <el-form-item label="英文單字" prop="english" required>
              <el-input v-model="fromData.english"/>
            </el-form-item>
            &emsp;
            <el-form-item label="中文翻譯" prop="chinese" required>
              <el-input v-model="fromData.chinese"/>
            </el-form-item>
            &emsp;
            <el-form-item>
              <el-button type="primary" @click="fromSubmit(ruleForm)">新增資料</el-button>
            </el-form-item>
          </el-row>
        </el-form>
      </el-container>
    </el-row>
    <el-row>
      <el-divider/>
      <el-text>TOEIC</el-text>
      &emsp;
      <el-text>隨機單字</el-text>
      <el-table :data="tableData" height="200px">
        <el-table-column width="80px">
          <template #default="scope">
            <el-button
                link type="primary"
                @click.prevent="editRow(scope.row)"
            >修改
            </el-button>
          </template>
        </el-table-column>
        <el-table-column
            prop="english"
            label="英文單字"
            width="200px"
        />
        <el-table-column
            prop="chinese"
            label="中文翻譯"
            width="200px"
        />
        <el-table-column
            prop="example"
            label="英文例句"
        />
        <el-table-column
            prop="explain"
            label="中文例句翻譯"
        />
      </el-table>
    </el-row>
    <el-row>
      <el-container>
        <el-form :model="fromData" :rules="rules" ref="ruleForm">
          <el-row>
            <el-form-item label="新增英文例句" prop="example" required>
              <el-input v-model="fromData.example" type="textarea" style="width: 500px"/>
            </el-form-item>
            &emsp;
            <el-form-item label="新增中文例句翻譯" prop="explain" required>
              <el-input v-model="fromData.explain" type="textarea" style="width: 500px"/>
            </el-form-item>
            &emsp;
            <el-form-item>
              <el-button type="primary" @click="fromSubmitEx(ruleForm)">新增資料</el-button>
            </el-form-item>
          </el-row>
        </el-form>
      </el-container>
    </el-row>

    <el-dialog v-model="dialogFormVisible" title="修改資料">
      <el-container>
        <el-form :model="fromData" :rules="dialogRules" ref="ruleForm">
          <el-row>
            <el-form-item label="英文單字" prop="dialogEnglish" required>
              <el-input v-model="fromData.dialogEnglish"/>
            </el-form-item>
            &emsp;
            <el-form-item label="中文翻譯" prop="dialogChinese" required>
              <el-input v-model="fromData.dialogChinese"/>
            </el-form-item>
          </el-row>
          <el-row>
            <el-form-item label="修改英文例句&emsp;&emsp;">
              <el-input v-model="fromData.dialogExample" type="textarea" style="width: 500px"/>
            </el-form-item>
            &emsp;
            <el-form-item label="修改中文例句翻譯">
              <el-input v-model="fromData.dialogExplain" type="textarea" style="width: 500px"/>
            </el-form-item>
          </el-row>
          <el-row>
            <el-form-item>
              <el-button type="primary" @click="setData">修改資料</el-button>
            </el-form-item>
          </el-row>
        </el-form>
      </el-container>
    </el-dialog>

  </div>

</template>

<style scoped>

</style>