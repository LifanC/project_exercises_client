<script setup>
import {reactive, ref} from "vue";
import axios from "axios";

const info = ref([])
const infoData = ref()
const num = ref()

const musicTen = ref([])
const musicNewSet = ref([])
const musicLength = ref(0)
const musicTenIndex = ref()
const valNum = ref(0)

const getMusicResponse = ref()
const tableData = ref()
const tableDataArr = ref([])


PubSub.subscribe('go', function (msg, data) {
  num.value = data[1].toString()
  if (data[0]) {
    info.value.splice(0,info.value.length)
    musicTen.value.splice(0,musicTen.value.length)
    musicNewSet.value.splice(0,musicNewSet.value.length)
    tableDataArr.value.splice(0,tableDataArr.value.length)
    fromData.musicTitle = ''
    getMusicRule()
    getMusic()
  }else{
    info.value.splice(0,info.value.length)
    musicTen.value.splice(0,musicTen.value.length)
    musicNewSet.value.splice(0,musicNewSet.value.length)
    tableDataArr.value.splice(0,tableDataArr.value.length)
    fromData.musicTitle = ''
  }
})

const fromData = reactive({
  musicTitle: ''
})

function getMusicRule() {
  axios.get('https://cloud.culture.tw/frontsite/trans/SearchShowAction.do?method=doFindTypeJOpenApi&category=' + num.value)
      .then((res) => {
        info.value.push({
          title: res.data.info.title,
          termsOfService: res.data.info.termsOfService,
          email: res.data.info.contact.email,
          name: res.data.info.license.name,
          url: res.data.info.license.url,
          version: res.data.info.version
        })
        infoData.value = info.value
      })
}




function getMusic() {
  axios({
    method: 'get',
    url: 'https://cloud.culture.tw/frontsite/trans/SearchShowAction.do?method=doFindTypeJ&category=' + num.value
  }).then((response) => {
    getMusicResponse.value = response.data
    for (let i = 0; i < response.data.length; i++) {
      musicNewSet.value.push(response.data[i].title)
    }
    // musicNewSet.value = [...new Set(musicPush.value)]
    musicLength.value = response.data.length
    paginationNum(1)
  })
}

function paginationNum(val) {
  fromData.musicTitle = ''
  valNum.value = val
  let ten = 10
  let a = ((val - 1) * ten)
  let b = (((val - 1) * ten) + ten)
  musicTen.value = musicNewSet.value.slice(a, b)
}

const endTime = ref()
const location = ref()
const locationName = ref()
const price = ref()
const time = ref()

const webSales = ref()
const imageUrl = ref()
const srcList = ref([])

function showSrcList() {
  srcList.value.splice(0,srcList.value.length)
  srcList.value.push(imageUrl.value)
}

function selectData() {
  musicTen.value.forEach((e, index) => {
    if (e === fromData.musicTitle) {
      musicTenIndex.value = index
    }
  })
  let ten = 10
  switch (valNum.value) {
    case 1:
      musicTenIndex.value += ((valNum.value - 1) * ten)
      break
    case 2:
      musicTenIndex.value += ((valNum.value - 1) * ten)
      break
    case 3:
      musicTenIndex.value += ((valNum.value - 1) * ten)
      break
    case 4:
      musicTenIndex.value += ((valNum.value - 1) * ten)
      break
    case 5:
      musicTenIndex.value += ((valNum.value - 1) * ten)
      break
  }
  tableData.value = getMusicResponse.value[musicTenIndex.value]
  tableDataArr.value.push(
      {
        UID: tableData.value.UID,
        category: tableData.value.category,
        descriptionFilterHtml: tableData.value.descriptionFilterHtml,
        discountInfo: tableData.value.discountInfo,
        endDate: tableData.value.endDate,
        imageUrl: tableData.value.imageUrl,
        masterUnit: tableData.value.masterUnit,
        showInfo: tableData.value.showInfo,
        showUnit: tableData.value.showUnit,
        sourceWebName: tableData.value.sourceWebName,
        startDate: tableData.value.startDate,
        title: tableData.value.title,
        version: tableData.value.version,
        webSales: tableData.value.webSales
      }
  )
  if (tableDataArr.value.length > 1) {
    (tableDataArr.value).shift()
  }
  endTime.value = tableDataArr.value[0].showInfo[0].endTime
  location.value = tableDataArr.value[0].showInfo[0].location
  locationName.value = tableDataArr.value[0].showInfo[0].locationName
  price.value = tableDataArr.value[0].showInfo[0].price
  time.value = tableDataArr.value[0].showInfo[0].time
  webSales.value = tableDataArr.value[0].webSales
  imageUrl.value = tableDataArr.value[0].imageUrl
}


</script>

<template>
  <el-divider/>
  <el-text>API說明文件</el-text>
  <el-container>
    <el-table :data="infoData" height="80px">
      <el-table-column
          prop="title"
          label="標題"
      />
      <el-table-column
          prop="termsOfService"
          label="服務條款"
      />
      <el-table-column
          prop="email"
          label="信箱"
      />
      <el-table-column
          prop="name"
          label="名稱"
      />
      <el-table-column
          prop="url"
          label="網址"
      />
      <el-table-column
          prop="version"
          label="版本"
      />
    </el-table>
  </el-container>
  <el-pagination
      @current-change="paginationNum"
      layout="prev, pager, next"
      :total="musicLength"
  />
  <el-container>
    <el-select
        v-model="fromData.musicTitle" filterable
        style="width: 50%"
        placeholder="選擇音樂表演資訊，第1頁、第2頁、第3頁....."
    >
      <el-option
          v-for="item in musicTen"
          :key="item"
          :label="item"
          :value="item"
          @click="selectData"
      />
    </el-select>
  </el-container>
  <el-container>
    <el-table :data="tableDataArr" style="width: 100%" height="100%">
      <el-table-column width="100" prop="startDate" label="開始日期"/>
      <el-table-column width="100" prop="endDate" label="結束日期"/>
      <el-table-column width="200" prop="sourceWebName" label="網站名稱"/>
      <el-table-column width="250" label="網絡銷售">
        <el-link
            :href="webSales"
            type="primary"
            target="_blank"
            underline
        >
          {{ webSales }}
        </el-link>
      </el-table-column>
      <el-table-column width="300" label="信息">
        時間 : {{ time }}
        <br>
        結束時間 : {{ endTime }}
        <br>
        地點名稱 : {{ locationName }}
        <br>
        地點 : {{ location }}
        <br>
        金額 : {{ price }}
      </el-table-column>
      <el-table-column width="200" prop="masterUnit" label="主單元"/>
      <el-table-column width="200" prop="showUnit" label="展示單位"/>
      <el-table-column width="500" prop="descriptionFilterHtml" label="描述"/>
      <el-table-column width="250" label="圖片">
        <el-image
            style="width: 250px;"
            :src="imageUrl"
            :preview-src-list="srcList"
            fit="cover"
            @click="showSrcList"
        />
      </el-table-column>
    </el-table>


  </el-container>

</template>

<style scoped>

</style>