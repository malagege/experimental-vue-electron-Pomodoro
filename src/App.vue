<template>
  <div id="app" :class="{mask: setTime_show}">
    <a href="javascript:;"  v-if="time_id==0" @click="setTime_show_true">設定</a>
    <SetTime class="fix_window" v-if="setTime_show" :d_time="d_time" :down_time="down_time" @ch_time="ch_time($event)" @close_setting="close_setting"></SetTime>
    <TimeDownShow v-model="time"></TimeDownShow>
    <transition name="a">
      <button v-if="h <= 0" @click="starttime" >Start Time</button>
      <button v-if="h > 0 && h < 100" @click="timeHandler">{{ is_stop_time ? 'Start' :　'Stop'}} Time</button>
      <button v-if="h >= 100" @click="dtime">Down Time</button>
    </transition>
    <TheBackground :h="h"></TheBackground>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import TheBackground from './components/TheBackground.vue'
import TimeCountDown from './components/TimeCountDown.vue'
import TimeDownShow from './components/TimeDownShow.vue'
import SetTime from './components/SetTime.vue'
let e_window = require('electron').remote.getCurrentWindow()
let desktopIdle = require('desktop-idle')

export default {
  name: 'app',
  data(){
    return{
      h: 0,
      time_id: 0,
      d_time: 1500,
      down_time: 300,
      check_idle_id:0,
      idle_time: 0,
      time: 0,
      setTime_show: false,
      time_function: null,
      is_stop_time: false,
      audio: new Audio(),
    }
  },
  methods:{
    timeHandler(){
      if(!this.is_stop_time){
        clearInterval(this.time_id)
        this.time_id = 0;
      }else{
        this.time_function();
      }
      this.is_stop_time = !this.is_stop_time;
    },
    ch_time(value){
      // this.time = value;
      this.d_time = value.d_time;
      this.down_time = value.down_time;

    if (this.h >= 100){
      this.time = this.down_time;
    }else{
      this.time = this.d_time;
    }

      this.close_setting()
    },
    close_setting(){
      this.setTime_show=false
    },
    setTime_show_true(){
      this.setTime_show = true
    },
    starttime(){
      clearInterval(this.check_idle_id);
      this.time_function = this.starttime;
      if(this.time_id){
        clearInterval(this.time_id)
        this.time_id = 0
        this.time = this.down_time
      }
      this.time_id = setInterval(()=>{
        // if(this.h >= 100 ){
        if(this.time == 0 ){
          this.h = 100
          clearInterval(this.time_id)
          this.time_id = 0;
          this.time = this.down_time
          this.notifyMe("take break ,go workaround!!!!!!!!!!")
          this.check_idle_id = setInterval(()=>{
            if(desktopIdle.getIdleTime() < 120 ){
              this.notifyMe2( (++this.idle_time) + " times, take break ,go workaround!!!!!!!!!!")
            }else{
              this.dtime()
              clearInterval(this.check_idle_id);
              this.check_idle_id = 0
              this.idle_time = 0
            }
          },120000)
          return false
        }
        this.time--
        this.h += this.add_time
      },1000);
    },
    test:function(){
      alert('test');
    },
    dtime(){
      clearInterval(this.check_idle_id);
      this.time_function = this.dtime;
      if(this.time_id){
        clearInterval(this.time_id)
        this.time_id = 0
        this.time = this.d_time
      }
      this.time_id = setInterval(()=>{
        // if(this.h <= 0 ){
        if(this.time == 0 ){
          this.h = 0
          clearInterval(this.time_id)
          this.time_id = 0;
          this.time_id = 0
          this.time = this.d_time
          this.notifyMe("GO WORK!!!!!!!!!!!!cheers!!!!")
          this.check_idle_id = setInterval(()=>{
            if(desktopIdle.getIdleTime() < 120 ){
              this.starttime();
              clearInterval(this.check_idle_id);
              this.check_idle_id = 0
              this.idle_time = 0
            }else{
              this.notifyMe2( (++this.idle_time) + " times, GO WORK!!!!!!!!!!!!cheers!!!!")
            }
          },120000)
          return false
        }
        this.time--
        this.h -= this.delete_time
      },1000);
    },
    notifyMe2(msg){
      // 首先讓我們確定瀏覽器支援 Notification
      if (!("Notification" in window)) {
        alert("這個瀏覽器不支援 Notification");
      }

      // 再檢查使用者是否已經授權執行 Notification
      else if (Notification.permission === "granted") {
        // 如果已經授權就可以直接新增 Notification 了!
        var notification = new Notification(msg);
      }

      // 否則，我們會需要詢問使用者是否開放權限
      else if (Notification.permission !== 'denied') {
        Notification.requestPermission(function (permission) {
          // 如果使用者同意了就來新增一個 Notification 打聲招呼吧
          if (permission === "granted") {
            var notification = new Notification(msg);
          }
        });
      }

      // 如果使用者還是不同意授權執行 Notification
      // 最好還是進行適當的處理以避免繼續打擾使用者
      e_window.flashFrame(true)
    },
    notifyMe(msg) {
    this.audio.src = 's.flac'
    this.audio.play();
    this.notifyMe2(msg)
}
  },
  created(){
    this.time = this.d_time;
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
    TheBackground,
    TimeDownShow,
    SetTime
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
.mask::before{
  content:'';
  display: block;
  width: 100vh;
  height: 100vh;
  position: fixed;
  z-index: 0;
  top:0px;
  background-color: rgba(0,0,0,.25);
}
.fix_window{
  position: fixed;
  max-height: 80%;
  overflow: auto;
  top: 20%;
  z-index: 99;
  background: white;
}
</style>
