<template>
  <!-- 滑动切换效果 -->
  <div class="container">
    <ul ref="ul" class="banner" @mouseover="onMouseover" @mouseout="onMouseout">
      <li v-for="item in arr" class="banner-item">
        <img :src="item.url" />
      </li>
      <!-- 这个作为最后一个切换时的过渡 -->
      <li class="banner-item">
        <img :src="arr[0].url" />
      </li>
    </ul>
    <ul class="indicator">
      <li
        class="indicator-item"
        :class="currentIndex == index ? 'active' : ''"
        v-for="(item, index) in arr"
        @click="onIndicatorClick(index)"
      >
        {{ index + 1 }}
      </li>
    </ul>
    <!-- 左右箭头 -->
    <span class="left floatLeft" @click="pre">&lt;</span>
    <span class="right floatRight" @click="next">&gt;</span>
  </div>
</template>
<script setup>
import { onMounted, ref } from "vue";
const ul = ref();
let marginLeft = 0;
const bannerWidth = 500;
let timer;
let currentIndex = ref(0);
const arr = [
  {
    url: "https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b8d231a74e149e690fef9bcffc96797~tplv-k3u1fbpfcp-zoom-crop-mark:1512:1512:1512:851.awebp?",
  },
  {
    url: "https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b8d465cbb594cc0b8ff42e218d19c40~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?",
  },
  {
    url: "https://img1.baidu.com/it/u=1960110688,1786190632&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=281",
  },
];
onMounted(() => {
  run();
});

const run = () => {
  if (marginLeft <= -arr.length * bannerWidth) {
    marginLeft = 0;
  }
  let animationTime = 10; //切换动画的速度
  if (marginLeft % bannerWidth == 0) {
    animationTime = 4000;
  }
  currentIndex.value = Math.round(-marginLeft / bannerWidth);
  ul.value.style.marginLeft = marginLeft + "px";
  marginLeft -= 10;
  timer = setTimeout(run, animationTime);
};

const pre = () => {
  let num = Math.floor(-marginLeft / bannerWidth) - 1;
  if (num === -1) {
    num = arr.length - 1;
  }
  console.log(num);
  jumpPage(num);
};
const next = () => {
  let num = Math.floor(-marginLeft / bannerWidth) + 1;
  if (num == arr.length) {
    num = 0;
  }
  jumpPage(num);
};

const jumpPage = (num) => {
  let _marginLeft = -(num * bannerWidth);
  ul.value.style.marginLeft = _marginLeft + "px";
  marginLeft = _marginLeft;
  console.log(marginLeft);
};

const onMouseover = () => {
  clearTimeout(timer);
};
const onMouseout = () => {
  run();
};
const onIndicatorClick = (index) => {
  currentIndex.value = index;
  jumpPage(index);
};
</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  position: relative;
  overflow: hidden;
  width: v-bind(bannerWidth + "px");
}
.banner {
  width: 2000px;
}
ul {
  list-style: none;
}
.banner-item {
  float: left;
  width: v-bind(bannerWidth + "px");
  height: 300px;
}
.banner-item img {
  width: v-bind(bannerWidth + "px");
  height: 300px;
  object-fit: fill;
  border: red solid 1px;
}
/* 左右箭头 */
.left,
.right {
  position: absolute;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  line-height: 30px;
  text-align: center;
  font-size: 20px;
  background-color: #999;
  color: white;
  cursor: pointer;
}
.floatLeft {
  top: 50%;
  left: 10px;
  transform: translateY(-50%);
}
.floatRight {
  top: 50%;
  transform: translateY(-50%);
  right: 10px;
}
/* 小圆点指示器 */
.indicator {
  position: absolute;
  right: 10px;
  bottom: 20px;
  z-index: 10;
}
.indicator .indicator-item {
  float: left;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  line-height: 30px;
  text-align: center;
  background-color: #999;
  color: white;
  margin-left: 5px;
  cursor: pointer;
}
.active {
  background-color: red !important;
}
</style>
