
<template>
  <el-form
    :model="ruleForm"
    status-icon
    ref="ruleForm"
    label-width="100px"
    class="demo-ruleForm"
  >
    <el-form-item label="服务器地址">
      <el-input autocomplete="off" v-model="host"></el-input>
    </el-form-item>
    <el-form-item label="用户名">
      <el-input autocomplete="off" v-model="username"></el-input>
    </el-form-item>
    <el-form-item label="密码">
      <el-input type="password" v-model="password"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  data() {
    return {
      host: "",
      username: "",
      password: "",
    };
  },
  methods: {
    submitForm() {
      let data = {
        host: this.host,
        username: this.username,
        password: this.password,
      };
      this.$emit("data", data);
    },
  },
};
</script>

<style>
</style>
