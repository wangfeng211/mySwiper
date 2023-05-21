<template>
  <div class="container">
    <ul class="banner" @mouseover="onMouseover" @mouseout="onMouseout">
      <li
        class="banner-item"
        v-for="(item, index) in arr"
        :class="currentIndex === index ? 'active' : ''"
      >
        <img :src="item.url" />
      </li>
    </ul>
    <!-- 小圆点 -->
    <ul class="indicator">
      <li
        v-for="(item, index) in arr"
        :key="item.url"
        class="indicator-item"
        :class="index === currentIndex ? 'active' : ''"
      >
        {{ index + 1 }}
      </li>
    </ul>

    <span class="arrow prev" @click="onPrev">&lt;</span>
    <span class="arrow next" @click="onNext">&gt;</span>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";

let timer;
const currentIndex = ref(0);
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

const onPrev = () => {
  stopTimer();
  currentIndex.value--;
  if (currentIndex.value === 0) {
    currentIndex.value = arr.length - 1;
  }
  startTimer();
};

const onNext = () => {
  stopTimer();
  currentIndex.value++;
  if (currentIndex.value === arr.length) {
    currentIndex.value = 0;
  }
  startTimer();
};
const stopTimer = () => {
  clearInterval(timer);
};
const startTimer = () => {
  timer = setInterval(() => {
    currentIndex.value++;
    if (currentIndex.value === arr.length) {
      currentIndex.value = 0;
    }
  }, 2000);
};
const onMouseover = () => {
  stopTimer();
};
const onMouseout = () => {
  startTimer();
};
onMounted(() => {
  startTimer();
});
</script>

<style scoped>
ul li {
  list-style: none;
}
.container {
  width: 500px;
  height: 300px;
  position: relative;
}
.banner {
  width: 100%;
  height: 100%;
}
.banner-item {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 1px solid red;
  opacity: 0;
  transition: all 0.8s;
}
.banner-item img {
  width: 100%;
  height: 100%;
  object-fit: fill;
}
.active {
  z-index: 10;
  opacity: 1 !important;
}
/* 上一页 下一页 */
.arrow {
  position: absolute;
  z-index: 11;
  top: 50%;
  transform: translateY(-50%);
  font-size: 18px;
  width: 30px;
  height: 40px;
  line-height: 40px;
  text-align: center;
  background-color: #999;
  color: white;
  cursor: pointer;
}
.prev {
  left: 10px;
}
.next {
  right: 10px;
}
.indicator {
  position: absolute;
  right: 10px;
  bottom: 20px;
  z-index: 11;
}
.indicator-item {
  float: left;
  margin-left: 10px;
  width: 30px;
  height: 30px;
  line-height: 30px;
  font-size: 15px;
  text-align: center;
  background-color: #999;
  color: white;
  border-radius: 50%;
}
.active {
  background-color: red !important;
}
</style>
