<template>
  <myUview :globalObj="globalObj" v-if="localObj.pageShow" />
  <myHeader :parentObj="localObj">
    <template #myHeaderSlot>
    </template>
  </myHeader>
  <view class="my-underground-purple" />
  <theTable :localObj="localObj" />
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
import { onLoad, onShow, onHide, onReachBottom } from "@dcloudio/uni-app";
import { fetchData, fakeData } from './components/data.js'; //引入数据

//组件引入
import myUview from "/src/components/my-uview/index.vue";
import myHeader from "/src/components/my-header/index.vue";
import theTable from './components/theTable.vue';

//父系入参
const { onNav, onNavBack, globalData } = globalThis.app;

const props = defineProps({
  globalObj: Object,
});

//本地变量和函数
let localObj = reactive({
  pageName: "文件管理",
  headerColor: "#9acafc",
  titleStyle: "font-size: 1.3rem !important; font-weight: bold; color: white !important;",
  showBackButton: true,
  pageShow: false,
  fn: {},
  users: {},
});

onLoad(async () => {
  await loadData({ options: ['users'] })
  loadData({ profiles: { userName: 'Fred' } })
})

onShow(async () => {
  localObj.pageShow = true;
});

onHide(() => {
  localObj.pageShow = false;
})

async function loadData({ options, profiles, folders } = {}) {
  if (options) {
    let optionArr = await fakeData({ options });
    if (!optionArr) return //globalThis.queryResult(false, '删除请求失败');
    setOptions(optionArr);
  }
  if (profiles) {
    let profileObj = await fakeData({ profiles });
    if (!profileObj) return //globalThis.queryResult(false, '删除请求失败');
    setProfiles(profileObj);
  }
  if (folders) {
    let folderArr = await fakeData({ folders });
    if (!folderArr) return //globalThis.queryResult(false, '删除请求失败');
    localObj.tables.setTables(folderArr);
  }
}

function setOptions(optionArr) {
  if (!optionArr) return //globalThis.queryResult(false, '获取选项失败');
  let finalOptionObj = {};
  for (let { tag, list } of optionArr) {
    finalOptionObj[tag] = [/* { name: '清空', label: "清空", text: '清空', ['选中']: true, value: "" }, */ ...list.map((obj) => {
      return { ...obj, label: obj.name, text: obj.name, value: obj.id }
    })]
  }
  localObj.options = finalOptionObj;
  console.log('选项', localObj.options);
}

function setProfiles(profileObj) {
  if (!profileObj) return //globalThis.queryResult(false, '获取选项失败');
  localObj.users = profileObj;
  console.log('用户信息', localObj.users);
  let { userId, currentFolderPath } = profileObj;
  loadData({ folders: { userId, currentFolderPath } });
}
</script>

<style></style>
