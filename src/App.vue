<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <input type="text" v-model="label_data">
    <input type="text" v-model="ip">
    <input type="text" v-model="port">
    <button @click="send">发消息</button>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    // shopId: ''
    return {
      shopId: '',
      messageInfo: '{"toUserId":"huxy","message":"i love you"}',
      label_data: 'label_data',
      ip: '127.0.0.1',
      port: '8080'
    }
  },
  created() { // 页面创建生命周期函数
    this.initWebSocket()
  },
  destroyed: function () { // 离开页面生命周期函数
    this.websocketclose();
  },
  methods: {
    collapse: function(){
      this.isCollapse = !this.isCollapse;
      if (this.isCollapse) {
        this.iconClass = "cebianlanzhankai";
      } else{
        this.iconClass = "cebianlanshouhui";
      }
    },
    initWebSocket: function () {
      // WebSocket与普通的请求所用协议有所不同，ws等同于http，wss等同于https
      this.websock = new WebSocket("ws://127.0.0.1:8080/websocket/wupx");
      this.websock.onopen = this.websocketonopen;
      this.websock.onerror = this.websocketonerror;
      this.websock.onmessage = this.websocketonmessage;
      this.websock.onclose = this.websocketclose;
    },
    websocketonopen: function () {
      console.log("WebSocket连接成功");
    },
    websocketonerror: function (e) {
      console.log(e);
      console.log("WebSocket连接发生错误");
    },
    websocketonmessage: function (e) {
      var da = e.data; // JSON.parse(e.data);
      console.log(da);
      this.message = da;
    },
    websocketclose: function (e) {
      console.log("connection closed (" + e.code + ")");
    },
    send: function () {
      // this.websock.send(params)
      /*if (this.websock instanceof WebSocket) {
        this.websocketclose({code: '0000'});
        this.websock.close();
      }*/
      console.log(this.messageInfo);
      /*this.websock = new WebSocket("ws://" + this.ip + ":" + this.port + "/websocket/wupx");
      this.websock.onopen = this.websocketonopen;
      this.websock.onerror = this.websocketonerror;
      this.websock.onmessage = this.websocketonmessage;
      this.websock.onclose = this.websocketclose;*/

      let data =  {
        toUserId: 'abc',
        messageInfo: '{"toUserId":"huxy","message":"i love you"}',
        label_data: this.label_data,
        ip: this.ip,
        port: this.port
      }
      this.websock.send(JSON.stringify(data));
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
