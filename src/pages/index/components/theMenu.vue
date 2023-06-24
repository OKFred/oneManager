<template>
  <view v-for="(arr, i) in localObj.menus.finalMenuArr" :key="i" style="padding: 0 5vw; " class="my-menu">
    <view style="padding: 3vh 0 1vh;" class="my-heading">{{ localObj.menus.finalTitleArr[i] }}</view>
    <view class="" style="background-color: white; padding: 1vh 0; border-radius: 0.5rem;">
      <view v-for="(item, index) in arr" :key="index" @tap="onMenu(item)">
        <view class="my-center my-center-vertically"
          :style="`flex-direction: column; padding-top: 1.5vh; ${arr.length && index < arr.length - 1 ? 'border-bottom: 1px dashed #ecf5ff' : ''}`">
          <image style="width: 2rem; height: 2rem" :src="item.src" mode="widthFix" />
          <view style="padding: 2vh 0; font-weight: bold; font-size: 0.93rem;">
            {{ item.label }}
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
//框架引入
import {
  reactive,
  watch,
  computed,
  onMounted,
  onActivated,
  onDeactivated,
  onUpdated,
  defineProps,
} from "vue";

//组件引入

//图片
import icon_browser from "/src/static/assets/icon_browser.svg";
import icon_folder from "/src/static/assets/icon_folder.svg";

//父系入参
const { onNav, onNavBack, globalData } = globalThis.app;

const props = defineProps({
  localObj: Object,
});

function setData(obj = {}) {
  Object.assign(props.localObj, obj);
  return props.localObj;
} //微信setData替代品

//本地变量和函数
setData({
  menus: {
    menuArr: [
      {
        parent: "本地",
        label: "文件管理",
        page: "本地-文件管理",
        src: icon_folder,
      },
      {
        parent: "远程",
        label: "信息助理",
        page: "远程-信息助理",
        src: icon_browser,
      },
    ],
    finalMenuArr: [],
    finalTitleArr: [],
  }
});

onMounted(() => {
  menuConfig();
});

function menuConfig() {
  let menuLocal = props.localObj.menus.menuArr.filter((obj) => obj.parent === "本地");
  let menuRemote = props.localObj.menus.menuArr.filter((obj) => obj.parent === "远程");
  props.localObj.menus.finalMenuArr = [menuLocal, menuRemote];
  props.localObj.menus.finalTitleArr = ["本地", "远程"]
}

function onMenu({ page, disabled, paramObj = {} } = {}) {
  if (disabled) return console.log("按钮已禁用");
  return onNav({ pageName: page });
}

</script>

<style>
.my-menu {
  display: flex;
  flex-direction: column;
  border-radius: 0.5rem;
  font-size: 1.2rem;
}

.my-col-2 {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
}

.my-col-3 {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.my-col-4 {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
</style>
