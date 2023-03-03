<template>
  <div class="model" :class="[visible ? 'model-show': '']"  @click.stop="setVisible(false)">
      <div class="container" @click.stop>
        <div class="title">视频演示</div>
        <div class="video-ref" ref="contentRef"></div>
      </div>
  </div>
</template>

<script>
import {reactive, toRefs, ref, nextTick} from "vue";

export default {
  name: "modelIndex",
  setup() {

    const contentRef = ref()
    const state = reactive({
      visible: false,

      setVisible: (visible = true, cb) => {
        state.visible = visible

        nextTick(() => {
          cb && cb(contentRef)
        })
      },

    })


    return {
      contentRef,
      ...toRefs(state)
    }


  }
}
</script>

<style scoped>
.model {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  transition: all .3s;
  transform: translateY(-1000px);

}

.model-show {
  transform: translateY(0);
}



.model::before {
  content: '';
  display: block;
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, .2);

}

.container {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 999;
  width: 80%;
  height: 90%;
  background-color: rgba(255, 255, 255, .9);
  border-radius: 10px;
  box-shadow: 0 0 15px #eee;
  color: #333;
  display: flex;
  flex-direction: column;
}



.video-ref {
  margin-top: 20px;
  flex: 1;
  overflow-y: auto;
}


:deep(.rr-player) {
  width: 100% !important;

}

:deep(.rr-player__frame) {
  width: 100% !important;

}

</style>