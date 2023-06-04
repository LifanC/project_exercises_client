<script setup>

</script>

<template>
  <!--  3時(列)4式(欄)-->
  <el-divider/>
  <el-text>Chapter2</el-text>
  <el-descriptions
      title="補充"
      direction="vertical"
      :column="4"
      border
  >
    <el-descriptions-item>
      簡單式
      <el-divider/>
      進行式
      <el-divider/>
      完成式
      <el-divider/>
      完成進行式
    </el-descriptions-item>
    <el-descriptions-item label="現在">
      2
      <el-divider/>
      2
      <el-divider/>
      2
      <el-divider/>
      2
    </el-descriptions-item>
    <el-descriptions-item label="過去">
      3
      <el-divider/>
      3
      <el-divider/>
      3
      <el-divider/>
      3
    </el-descriptions-item>
    <el-descriptions-item label="未來">
      4
      <el-divider/>
      4
      <el-divider/>
      4
      <el-divider/>
      4
    </el-descriptions-item>
  </el-descriptions>

</template>

<style scoped>

</style>