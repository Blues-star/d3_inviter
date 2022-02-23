<script lang="ts" setup>
// This starter template is using Vue 3 <script setup> SFCs

import { ref, nextTick } from 'vue';
import { onMounted, onUpdated, onActivated } from 'vue';
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup

const b_h = 0.50;
const b_w = 0.50;
const img_w = 1240;
const img_h = 2208;
const fontsize = ref(140);
const oriw = ref(0)
const orih = ref(0)
const ImgRef = ref(null)
let leftsize = ref(147);
let topsize = ref(409);
let heightsize = ref(0);
let widthsize = ref(0);
let name = ref('');
let _show = ref(false);

const v_can = <HTMLCanvasElement> document.createElement('canvas');

v_can.height = img_h;
v_can.width = img_w;

function draw() {
  let wm = name.value;
  let imgref = <HTMLImageElement>document.getElementById("yqhimg");
  let v_ctx = v_can.getContext('2d');
  let b = document.documentElement.clientHeight / 2208;

  v_ctx.drawImage(imgref, 0, 0);
  //_show.value = false;
  v_ctx.font = 'bold ' + fontsize.value + 'px Arial';
  v_ctx.textAlign = 'center';
  let text = v_ctx.measureText(wm);
  let start_w = b_w * img_w;
  let start_h = b_h * img_h;

  console.log(start_w, start_h);

  v_ctx.fillText(wm, start_w, start_h);



  let dataURL = v_can.toDataURL('image/png', 100);

  let canvas = <HTMLCanvasElement> document.getElementById('mycanvas');
  let ctx = canvas.getContext('2d');


  
  let clientheight = document.documentElement.clientHeight;
  console.log(imgref.width, imgref.height);
  //let b = clientheight / 2208;
  let curw = b * img_w;
  let curh = clientheight;
  canvas.width = curw;//设置canvas容器宽度
  canvas.height = curh;

  let t = new Image();
  t.src = dataURL;

  ctx = canvas.getContext('2d');
  ctx.drawImage(t, 0, 0, curw, curh)
}

function save() {
  _show.value = false;
  let wm = name.value;
  let canvas = <HTMLCanvasElement>document.createElement('canvas');
  let ctx = canvas.getContext('2d');
  let imgref = <HTMLImageElement>document.getElementById("yqhimg");
  canvas.width = imgref.width;//设置canvas容器宽度
  canvas.height = imgref.height;//设置canvas容器高度
  let clientheight = document.documentElement.clientHeight;
  let b = clientheight / 2208;
  ctx.drawImage(imgref, 0, 0);
  //console.log(fontsize.value / b);
  let temp = 'bold ' + Math.ceil(fontsize.value / b) + 'px Arial'
  console.log(temp);
  ctx.font = temp;
  let text = ctx.measureText(wm);
  let start_w = b_w * imgref.width - text.width / 2;
  let start_h = b_h * imgref.height;
  ctx.fillText(wm, start_w, start_h);

  let dataURL = canvas.toDataURL('image/png', 100);
  //let img = document.createElement('img');
  //img.src = dataURL;
  //document.body.appendChild(img);
  //download image
  let a = document.createElement('a');
  a.href = dataURL;
  a.download = 'invite.png';
  document.body.appendChild(a);
  a.click();
  a.remove();

}

</script>

<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <!-- <HelloWorld msg="Hello Vue 3 + Vite" /> -->
  <!-- <n-space vertical size="large" class="space tail"> -->
  <n-layout has-sider class="side tail" content-style="height: 100%;">
    <n-layout-sider content-style="padding: 24px;height: 100%;" class="tail">
      <n-space vertical size="large">
        <n-layout>
          <n-input size="large" v-model:value="name" @input="draw" round placeholder="战队名称" />
        </n-layout>
        <n-layout>
          <n-input-number v-model:value="fontsize" @update:value="draw" />
        </n-layout>
        <n-layout style="height: 42px;">
          <n-space>
            <n-button @click="draw">绘制</n-button>
            <n-button @click="save">下载</n-button>
          </n-space>
        </n-layout>
      </n-space>
    </n-layout-sider>
    <n-layout class="tail rg" style="position:relative">
      <img id="yqhimg" ref="ImgRef" class="ni" src="./assets/yqh.png" />
      <canvas id="mycanvas" width="940" height="570"></canvas>
      <!-- <n-image
    width="100"
    src="https://07akioni.oss-cn-beijing.aliyuncs.com/07akioni.jpeg" show-toolbar-tooltip="true"
      />-->
      <!-- <div
        class="text"
        v-bind:style="{
          top: topsize + 'px', left: leftsize + 'px', height: heightsize + 'px', width:
            widthsize + 'px'
        }"
      >123456</div>-->
    </n-layout>
  </n-layout>
  <!-- </n-space> -->
</template>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #1c6bbb;
  margin-top: 60px;
  min-height: 100vh;
} */
html,
body,
#app {
  height: 100%;
}
.tail {
  height: 100%;
}
.side {
  background: #1c5488;
  height: 100%;
}
.body {
  height: 100%;
}
.ni {
  height: 100%;
  z-index: 5;
  display: none;
}
.text {
  position: absolute;
  z-index: 0;
  background-color: rgba(172, 114, 114, 0.555);
  text-align: center;
  vertical-align: middle;
  min-width: none;
}
</style>
