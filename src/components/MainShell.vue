
<template>
  <div style="margin-bottom: 20px">
    <el-button size="small" @click="addTab(editableTabsValue)">
      新建ssh终端
    </el-button>
  </div>
  <el-tabs
    v-model="editableTabsValue"
    type="card"
    closable
    @tab-remove="removeTab"
  >
    <el-tab-pane
      v-for="item in editableTabs"
      :key="item.name"
      :label="item.title"
      :name="item.name"
    >
      <!-- 失活的组件将会被缓存！-->
      <!-- <keep-alive> -->
      <component
        :is="item.content"
        :wsurl="'ws://localhost:8081/webssh'"
        :username="item.username"
        :password="item.password"
        :addr="item.host"
      ></component>
      <!-- </keep-alive> -->
    </el-tab-pane>
  </el-tabs>

  <el-dialog title="外层 Dialog" v-model="outerVisible" destroy-on-close>
    <NewShell v-on:data="data"></NewShell>
  </el-dialog>
</template>

<script>
import GoShell from "./GoShell.vue";
import NewShell from "./NewShell.vue";

export default {
  components: {
    GoShell,
    NewShell,
  },
  data() {
    return {
      outerVisible: false,
      editableTabsValue: "0",
      editableTabs: [],
      tabIndex: 0,
    };
  },
  methods: {
    data: function (data) {
      this.outerVisible = false;
      console.log(data);
      let newTabName = ++this.tabIndex + "";
      this.editableTabs.push({
        host: data.host,
        title: data.host,
        name: data.host + newTabName,
        username: data.username,
        password: data.password,
        content: GoShell,
      });
      this.editableTabsValue = data.host + newTabName;
    },
    addTab() {
      this.outerVisible = true;
    },
    removeTab(targetName) {
      let tabs = this.editableTabs;
      let activeName = this.editableTabsValue;
      if (activeName === targetName) {
        tabs.forEach((tab, index) => {
          if (tab.name === targetName) {
            let nextTab = tabs[index + 1] || tabs[index - 1];
            if (nextTab) {
              activeName = nextTab.name;
            }
          }
        });
      }

      this.editableTabsValue = activeName;
      this.editableTabs = tabs.filter((tab) => tab.name !== targetName);
    },
  },
};
</script>

<style>
</style>
