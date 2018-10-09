<template>
  <div id="app">
    {{h}}
    <transition name="a">
      <button v-if="h <= 0" @click="starttime" >Start Time</button>
      <button v-if="h >= 100" @click="dtime">Down Time</button>
    </transition>
    <TheBackground :h="h"></TheBackground>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import TheBackground from './components/TheBackground.vue'

let electron = require('electron');
let remote = electron.remote;
let e_window =  electron.remote.getCurrentWindow();
export default {
  name: 'app',
  data(){
    return{
      h: 0,
      time_id: 0,
      d_time: 1500,
      down_time: 300,
    }
  },
  methods:{
    starttime(){
      if(this.time_id){
        clearInterval(this.time_id)
        this.time_id = 0
      }
      this.time_id = setInterval(()=>{
        if(this.h >= 100 ){
          clearInterval(this.time_id)
          this.time_id = 0;
          this.notifyMe("take break ,go workaround!!!!!!!!!!")
          e_window.flashFrame(true)
          return false
        }
        this.h += this.add_time
      },1000);
    },
    dtime(){
      if(this.time_id){
        clearInterval(this.time_id)
        this.time_id = 0
      }
      this.time_id = setInterval(()=>{
        if(this.h <= 0 ){
          clearInterval(this.time_id)
          this.time_id = 0;
          this.notifyMe("GO WORK!!!!!!!!!!!!cheers!!!!")
          e_window.flashFrame(true)
          return false
        }
        this.h -= this.delete_time
      },1000);
    },
    notifyMe(msg) {
    let a = new Audio('s.flac');
    a.play();
  if (!("Notification" in window)) {
    alert( msg );
  }

  // 再檢查使用者是否已經授權執行 Notification
  else if (Notification.permission === "granted") {
    // 如果已經授權就可以直接新增 Notification 了!
    var notification = new Notification('通知', {
    body: msg
  })
  }

  // 否則，我們會需要詢問使用者是否開放權限
  else if (Notification.permission !== 'denied') {
    Notification.requestPermission(function (permission) {
      // 如果使用者同意了就來新增一個 Notification 打聲招呼吧
      if (permission === "granted") {
        var notification = new Notification('通知', {
    body: msg
  })
      }
    });
  }

}
  },
  computed:{
    add_time(){
      return  1 / this.d_time * 100 
    },
    delete_time(){
      return 1 / this.down_time * 100
    }
  },
  components: {
    HelloWorld,
    TheBackground
  }
}
</script>

<style>
html,body{
  margin:0px;
  padding:0px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
