<template>
  <div id="xterm" class="xterm" />
</template>

<script >
import { Terminal } from "xterm";
import "xterm/css/xterm.css";
import { FitAddon } from "xterm-addon-fit";
// import { AttachAddon } from "xterm-addon-attach";

export default {
  props: {
    wsurl: {
      type: String,
      default: "ws://localhost:8081/test?aaa",
    },
    username: {
      type: String,
      default: "root",
    },
    password: {
      type: String,
      default: "Password",
    },
  },
  mounted() {
    this.initSocket();
  },
  beforeUnmount() {
    this.socket.close();
    this.term.dispose();
  },
  methods: {
    initTerm() {
      console.log("xterm 初始化");
      const term = new Terminal({
        fontSize: 14,
        cursorBlink: true,
      });
      const fitAddon = new FitAddon();
      term.loadAddon(fitAddon);
      let container = document.getElementById("xterm");
      container.style.height = window.innerHeight + "px";
      term.open(container);
      fitAddon.fit();
      term.focus();
      this.term = term;
      // 获取终端第一次自适应的rows和cols属性
      console.log(this.term.cols, this.term.rows);
      // this.socket.send()
      // 监听窗口大小变化，设置xterm自适应浏览器大小
      window.onresize = () => {
        container.style.height = window.innerHeight + "px";
        fitAddon.fit();
      };
      this.term.onData((value) => {
        console.log("onData=:", value);
        let cmdData = {
          type: "cmd",
          cmd: value,
        };
        this.socket.send(JSON.stringify(cmdData));
      });
      this.term.onResize((size) => {
        let resizeData = {
          type: "resize",
          cols: size.cols,
          rows: size.rows,
        };
        console.log(resizeData);
        this.socket.send(JSON.stringify(resizeData));
      });
    },
    initSocket() {
      this.socket = new WebSocket(this.wsurl);
      this.socketOnClose();
      this.socketOnOpen();
      this.socketOnError();
      this.socketOnMessage();
    },
    socketOnMessage() {
      this.socket.onmessage = (a) => {
        this.term.write(a.data);
      };
    },
    socketOnOpen() {
      this.socket.onopen = () => {
        console.log("socket 连接成功");
        this.initTerm();
      };
    },
    socketOnClose() {
      this.socket.onclose = () => {
        console.log("socket 关闭");
      };
    },
    socketOnError() {
      this.socket.onerror = () => {
        console.log("socket 连接失败");
      };
    },
  },
};
</script>

<style>
</style>