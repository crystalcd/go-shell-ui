<template>
  <el-container>
    <el-aside width="200px">Aside</el-aside>
    <el-container>
      <el-header>Header</el-header>
      <el-main>
        <MainShell></MainShell>
        <div class="container"></div>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import MainShell from "./components/MainShell.vue";

export default {
  name: "App",
  components: {
    MainShell,
  },
};
</script>

<style>
</style>
