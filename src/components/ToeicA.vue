<script setup>
import {reactive, ref} from "vue";
import axios from "axios";

const show = ref()
const tableData = ref()
const exampleAndexplain = ref(true)
const elTableHeight = ref(100)

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
  queryEnglish: null,
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
  if(!fromData.queryEnglish){
    if (!ruleForm) return
    ruleForm.value.validate((valid) => {
      if (valid) {
        ElMessageBox.confirm(
            `確定要 新增 嗎?`,
            {
              confirmButtonText: '確定',
              cancelButtonText: '取消',
              type: 'info',
            }
        )
            .then(() => {
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
            })
            .catch(() => {
              ElMessage({
                message: '取消',
                grouping: true,
                type: 'error',
              })
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
  }else{
    ElMessage({
      message: '查詢單字 請清空',
      grouping: true,
      type: 'error',
    })
  }
}

function editRow(row) {
  dialogFormVisible.value = true
  fromData.toeicId = row.toeicId
  fromData.dialogEnglish = row.english
  fromData.dialogChinese = row.chinese
  fromData.dialogExample = row.example
  fromData.dialogExplain = row.explain
}

function queryExample(row) {
  window.open('https://dictionary.cambridge.org/zht/%E8%A9%9E%E5%85%B8/%E8%8B%B1%E8%AA%9E-%E6%BC%A2%E8%AA%9E-%E7%B9%81%E9%AB%94' + '/' + row.english)
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

function queryToeicWords() {
  if(!fromData.queryEnglish){
    exampleAndexplain.value = true
    elTableHeight.value = 100
  }else{
    exampleAndexplain.value = false
    elTableHeight.value = 500
  }
  axios.get('http://localhost:8080/queryToeicWords', {
    params: {
      english: fromData.queryEnglish
    }
  }).then((res) => {
    tableData.value = res.data
  })
}


</script>

<template>
  <div v-show="show">
    <el-divider/>
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
    <el-divider/>
    <el-row>
      <el-container>
        <el-form :model="fromData">
          <el-form-item label="查詢單字">
            <el-input @input="queryToeicWords" v-model="fromData.queryEnglish" clearable/>
          </el-form-item>
        </el-form>
      </el-container>
    </el-row>
    <el-divider/>
    <el-text>隨機單字</el-text>
    <el-table :data="tableData" :height="elTableHeight">
      <el-table-column width="80px">
        <template #default="scope">
          <el-button
              link type="primary"
              @click.prevent="editRow(scope.row)"
          >修改
          </el-button>
          <br>
          <el-button
              link type="primary"
              @click.prevent="queryExample(scope.row)"
          >查詢例句
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
    <div v-show="exampleAndexplain">
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
    </div>
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
            <el-form-item label="新增英文例句&emsp;&emsp;">
              <el-input v-model="fromData.dialogExample" type="textarea" style="width: 500px"/>
            </el-form-item>
            &emsp;
            <el-form-item label="新增中文例句翻譯">
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