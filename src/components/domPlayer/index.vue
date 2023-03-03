<template>

  <div class="title">模拟绘制demo视屏</div>

  <div>
    <button class="w-button" @click="created">录制</button>
    <button class="w-button" @click="preview">预览</button>
  </div>

  <div>
    <blockquote>暂不支持iframe</blockquote>
  </div>

  <div>
    <form action="">
      <label for=""><span class="star">用户名</span> <input type="text" placeholder="用输入用户名"/></label>
      <label for=""><span class="star">密码</span> <input type="text" placeholder="用输入密码"/></label>
      <label for=""><span class="star">邮箱</span> <input type="text" placeholder="用输入邮箱"/></label>
      <label for=""><span class="star">手机号</span> <input type="text" placeholder="用输入手机号"/></label>
      <div class="os-handler">
        <button class="w-button" type="submit">提交</button>
        <button class="w-button" type="reset">重置</button>
      </div>
    </form>
  </div>


  <model-index ref="rePlayer" />

  <mark-index ref="timerRef" />

  <draw-index ref="drawRef" />
</template>

<script  setup>
import {ref} from "vue";
const rWeb = require('rrweb')
import rWebPlayer from "rrweb-player";
import ModelIndex from "@/components/domPlayer/model.vue";
import MarkIndex from "@/components/mark/index.vue";
import DrawIndex from "@/components/draw/index.vue";

const rePlayer = ref()
const timerRef = ref()
const drawRef = ref()

let stopFun = ref(null)
const events = ref([[]])

const created = () => {
  timerRef.value?.setVisible(true, ()=> {
    stopFun = rWeb.record({
      checkoutEveryNth: 1000,
      emit(event, isCheckout) {
        // isCheckout 是一个标识，告诉你重新制作了快照
        if (isCheckout) {
          events.value.push([]);
        }

        if (events.value) {
          const lastEvents = events.value[events.value.length - 1];
          lastEvents.push(event);
        }
      },
      plugins: [
        rWeb.getRecordConsolePlugin({
          level: ["info", "log", "warn", "error"],
          lengthThreshold: 10000,
          stringifyOptions: {
            stringLengthLimit: 1000,
            numOfKeysLimit: 100,
            depthOfLimit: 1
          },
          logger: window.console,
        })
      ],
    })
  })

}
const preview = () => {
  // console.log('最近的操作记录: ', JSON.stringify(events.value[events.value.length - 1]));
  if(events.value[events.value.length - 1].length <= 0) {
    return alert("请先点击录制按钮进行录制！");
  }

  stopFun();

  rePlayer.value?.setVisible(true, (domRef) => {

    new rWebPlayer({
      target: domRef.value, // 可以自定义 DOM 元素
      // 配置项
      props: {
        logConfig: true,
        events: events.value[events.value.length - 1],
        plugins: [
          rWeb.getReplayConsolePlugin({
            level: ['info', 'log', 'warn', 'error'],
          }),
        ],
      },
    });
  })



}

</script>

<script>
export default  {
  name: "demoIndex"
}
</script>

<style scoped>

form {
  padding: 20px;
  display: flex;
  align-items: center;
  flex-direction: column;
}

label {
  margin-bottom: 20px;
}

label span {
  display: inline-block;
  width: 80px;
  padding: 0 14px;
  text-align: right;
}

label span.star::before {
  content: '*';
  display: inline-block;
  color: #F56C6C;
  vertical-align: middle;
  margin-right: 4px;
}

input {
  display: inline-block;
  width: 200px;
  padding: 10px 14px;
  border: 1px solid transparent;
  transition: all .3s;
  cursor: pointer;
  border-radius: 6px;
}

input:hover {
  border: 1px solid #646cff;
}
input:focus,
input:focus-visible {
  outline: none;
}

.os-handler {
  display: flex;
  width: 230px;
  padding-left: 110px;
}

.os-handler button {
  flex: 1;
  margin-right: 0;
  padding: 6px 14px;
}

.os-handler button:first-of-type {
  margin-right: 14px;
}

.os-handler button:last-of-type {
  background-color: transparent;
  border-color: #999;;
  color: #999;
}
</style>