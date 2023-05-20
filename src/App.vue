<script setup>
import AppAside from "@/components/AppAside.vue";
import AppMain from "@/components/AppMain.vue";

</script>

<template>
  <el-container>
    <el-container>
      <AppAside />
      <el-container>
        <el-header ></el-header>
        <AppMain />
      </el-container>
    </el-container>
  </el-container>
</template>

<style scoped>

</style>

