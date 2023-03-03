<template>
  <div class="mark-time" :class="[visible ? 'model-show': 'model-hide']" >
    <div class="timer">{{ time }}</div>
  </div>
</template>

<script>
import {reactive, toRefs} from "vue";

export default {
  name: "MarkIndex",

  setup() {

    const state = reactive({
      time: 3,
      timer: {},

      visible: false,

      setVisible: (visible = true, cb) => {
        state.visible = visible

        state.visible && cb && state.setTimer(cb)
      },

      setTimer: (cb) => {
        state.timer = setInterval(() => {
          state.time--
          if (state.time <=0) {
            clearInterval(state.timer)
            state.visible = false
            state.time = 3
            cb && cb()
          }
        }, 1000)
      }
    })



    return {
      ...toRefs(state)
    }
  }
}
</script>

<style scoped>
.mark-time {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 10;
  background-color: rgba(0, 0,0, 0.4);
  transform: scale(0);
  transition: all .5s;
}

.model-show {
  display: block;
  transform: scale(1);
}

.timer {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 100px;
  height: 100px;
  background-color: rgba(0, 0, 0, .6);
  color: #646cff;
  border-radius: 50px;
  font-size: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>