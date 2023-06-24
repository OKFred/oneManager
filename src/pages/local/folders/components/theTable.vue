<template>
  <view>
    <myTable :parentObj="{ ...localObj.tables, onTable }" />
  </view>
</template>

<script setup>
//框架引入
import {
  reactive,
  watch,
  computed,
  onMounted,
  onBeforeUnmount,
  onActivated,
  onDeactivated,
  onUpdated,
  getCurrentInstance,
  defineProps,
} from "vue";

//组件引入
import myTable from "/src/components/my-table/index.vue";
import icon_folder from "/src/static/assets/icon_folder.svg";
import icon_file from "/src/static/assets/icon_file.svg";
import icon_rar from "/src/static/assets/icon_rar.svg";


//父系入参
const { onNav, onNavBack, globalData } = globalThis.app;


const props = defineProps({
  localObj: Object,
});

function setData(obj) {
  Object.assign(props.localObj, obj);
}

//本地变量和函数
setData({
  tables: {
    setTables,
    tableRow: [],
    tableColumn: [
      { label: "文件名", thStyle: 'min-width: 15rem', tdStyle: 'min-width: 15rem', prefix: icon_folder },
      { label: '文件大小', },
      { label: "修改时间", thStyle: 'min-width: 10rem', tdStyle: 'min-width: 10rem',  },
    ],
  }
});

function setTables(arr) {
  if (!arr || (Array.isArray(arr) && arr.length === 0)) return;
  let newArr = arr.map((item, index) => {
    return {
      ...item,
      ['文件名']: item['fileName'],
      ['文件大小']: item['fileSize'],
      ['修改时间']: item['timeModified'],
    }
  });
  props.localObj.tables.tableRow = newArr;
  return newArr;
}

function onTable({ rowObj, row, col, colObj }) {
  if (colObj.key !== '查看') return;
  console.log('点击了查看', rowObj, row, col);
}


</script>

<style></style>
