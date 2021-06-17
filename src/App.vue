<template>
  <GoShell
    :wsurl="'ws://localhost:8081/test'"
    :username="'root'"
    :password="'Password1!'"
  ></GoShell>
</template>

<script>
import GoShell from "./components/GoShell.vue";

export default {
  name: "App",
  components: {
    GoShell,
  },
};
</script>

<style>
</style>
