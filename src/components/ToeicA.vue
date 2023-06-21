<script setup>
import {reactive, ref} from "vue";
import axios from "axios";

const show = ref()
const tableData = ref()
const exampleAndexplain = ref(true)
const elTableHeight = ref(100)

const fromData = reactive({
  english: '',
  englishAdd: '',
  chinese: '',
  chineseAdd: '',
  example: '',
  explain: '',
  toeicId: null,
  dialogEnglish: '',
  dialogChinese: '',
  dialogExample: '',
  dialogExplain: '',
  queryEnglish: '',
})


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
  axios.get('http://localhost:8080/toeic/toeicWords')
      .then((response) => {
        if (response.data.length === 0) {
          exampleAndexplain.value = false
        } else {
          exampleAndexplain.value = true
        }
        elTableHeight.value = 100
        tableData.value = response.data
      })
}

function fromSubmit() {
  axios({
    method: 'post',
    url: 'http://localhost:8080/toeic/toeicFromSubmit',
    data: {
      english: fromData.englishAdd
    }
  }).then((response) => {
    ElMessage({
      message: '成功',
      grouping: true,
      type: 'success',
    })
    fromData.englishAdd = null
    fromData.chineseAdd = null
  })
}

const dialogFormVisible = ref(false)

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
  if (fromData.dialogEnglish !== '') {
    axios({
      method: 'put',
      url: 'http://localhost:8080/toeic/setData',
      data: {
        toeicId: fromData.toeicId,
        english: fromData.dialogEnglish,
        chinese: fromData.dialogChinese,
        example: fromData.dialogExample,
        explain: fromData.dialogExplain
      }
    }).then((response) => {
      ElMessage({
        message: '成功',
        grouping: true,
        type: 'success',
      })
      tableData.value = response.data
      dialogFormVisible.value = false
    })
  } else {
    ElMessage({
      message: '英文單字，請勿空白',
      grouping: true,
      type: 'error',
    })
  }
}

function queryToeicWords() {
  if (!fromData.queryEnglish) {
    exampleAndexplain.value = true
    elTableHeight.value = 100
  } else {
    exampleAndexplain.value = false
    elTableHeight.value = 500
  }
  axios.get('http://localhost:8080/toeic/queryToeicWords', {
    params: {
      english: fromData.queryEnglish
    }
  }).then((response) => {
    tableData.value = response.data
  })
}

const tf = ref(true)

function inputEn() {
  if (fromData.englishAdd !== '') {
    axios.get('http://localhost:8080/toeic/tf', {
      params: {
        english: fromData.englishAdd
      }
    })
        .then((response) => {
          tf.value = response.data
          if (response.data) {
            ElMessage.error('單字重複')
          }
        })
  }
}

function all() {
  axios.get('http://localhost:8080/toeic/all')
      .then((response) => {
        exampleAndexplain.value = false
        elTableHeight.value = 500
        tableData.value = response.data
      })
}


</script>

<template>
  <div v-show="show">
    <el-divider/>
    <el-row>
      <el-container>
        <el-form :model="fromData">
          <el-row>
            &emsp;
            <el-form-item label="新增英文單字" prop="englishAdd">
              <el-input v-model="fromData.englishAdd" @input="inputEn" clearable/>
            </el-form-item>
            &emsp;
            <el-form-item>
              <el-button :disabled="tf" plain type="primary" @click="fromSubmit">確定</el-button>
            </el-form-item>
          </el-row>
        </el-form>
      </el-container>
    </el-row>
    <el-container>
      <el-form :model="fromData">
        <el-row>
          &emsp;
          <el-form-item label="查詢單字">
            <el-button link type="primary" @click="all">全部</el-button>
          </el-form-item>
        </el-row>
        <el-row>
          &emsp;
          <el-form-item label="查詢單字">
            <el-input @input="queryToeicWords" v-model="fromData.queryEnglish" clearable/>
          </el-form-item>
        </el-row>
      </el-form>
    </el-container>
    <el-table :data="tableData" :height="elTableHeight" border>
      <el-table-column width="100px" label="隨機單字">
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

    <el-dialog v-model="dialogFormVisible" title="修改資料">
      <el-container>
        <el-form :model="fromData">
          <el-row>
            <el-form-item label="英文單字" prop="dialogEnglish">
              <el-input v-model="fromData.dialogEnglish"/>
            </el-form-item>
            &emsp;
            <el-form-item label="中文翻譯" prop="dialogChinese">
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