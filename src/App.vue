<template>
  <div class="box">
    <div class="container">
      <div class="row">
        <!-- 数据 -->
        <div class="col-sm-4">
          <div class="panel panel-info">
            <div class="panel-heading">
              <!-- 全选按钮 -->
              <input type="checkbox" v-model="checkAll" @change="checkAllChange" :disabled="leftData.length == 0" />全选/全不选
              <span class="pull-right">{{ numChencked }}/{{ leftData.length }}</span>
            </div>
            <div class="panel-body">
              <ul>
                <li v-for="(v, i) in leftData" :key="i">
                  <div class="checkbox">
                    <label>
                      <input type="checkbox" v-model="v.check" @change="checkChange" :disabled="v.disable==true" />{{ v.name }}
                    </label>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <!-- 按钮 -->
        <div class="col-sm-2 text-center">
          <div>
            <button class="btn btn-info" v-on:click="toLeft">右</button>
            <button class="btn btn-info" v-on:click="toRight">左</button>
          </div>
        </div>
        <!-- 目标数据 -->
        <div class="col-sm-4">
          <div class="panel panel-info">
            <div class="panel-heading">
              <!-- 全选按钮 -->
              <input type="checkbox" v-model="checkAll1" @change="checkAllChange1" :disabled="rightData.length == 0"/>全选/全不选
              <span class="pull-right">{{ numChencked1 }}/{{ rightData.length }}</span>
            </div>
            <div class="panel-body">
              <ul>
                <li v-for="(v, i) in rightData" :key="i">
                  <div class="checkbox">
                    <label>
                      <input type="checkbox" v-model="v.check" @change="checkChange1" />{{ v.name }}
                    </label>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
// 全选状态
let checkAll = ref(false);
let checkAll1 = ref(false);
// 左框数据
let leftData = ref([
  { name: "选项一", check: false, disable: false },
  { name: "选项二", check: false, disable: false },
  { name: "选项三", check: false, disable: false },
  { name: "选项四", check: false, disable: true },
  { name: "选项五", check: false, disable: false },
  { name: "选项六", check: false, disable: false },
]);
// 右框数据
let rightData = ref([]);
// 计算选中的数据并显示（左）
// const numChencked = computed(() => {
//     // return data => data.value.filter((v) => v.check).length;
//     return function(data) {
//       return data.value.filter((v) => v.check).length;
//     }
// });
const numChencked = computed(() => {
  return leftData.value.filter((v) => v.check).length;
});
// 计算选中的数据并显示（右）
const numChencked1 = computed(() => {
  return rightData.value.filter((v) => v.check).length;
});
// 向右穿梭
function toRight() {
  // 将check = true的选中
  let selectD = leftData.value.filter((v) => v.check);
  // 追加数据
  rightData.value.push(...selectD);
  leftData.value = leftData.value.filter((v) => !v.check);
  // 穿梭后将check状态改为false
  rightData.value.map((item) => {
    if (item.check) {
      item.check = false;
    }
    return item;
  });
  checkAll.value = false;
}
// 向左穿梭
function toLeft() {
  // 将check = true的选中
  let selectD = rightData.value.filter((v) => v.check);
  // 追加数据
  leftData.value.push(...selectD);
  rightData.value = rightData.value.filter((v) => !v.check);
  // 穿梭后更改check的值
  leftData.value.map((item) => {
    if (item.check) {
      item.check = false;
    }
    return item;
  });
  checkAll1.value = false;
}
// 全选功能（左） 包括屏蔽disable = true 项
function checkAllChange() {
  leftData.value.map((item) => {
    if (checkAll.value) {
      item.check = !item.disable;
    }
    else if(!checkAll.value) {
      item.check = false;
      // console.log(item.check);
    }
    return item;
  });
}
// 全选功能（右） 
function checkAllChange1() {
  rightData.value.forEach((v) => (v.check = checkAll1.value));
}
// 子选项改变全选状态（左）
function checkChange() {
  // 子选项选中的个数
  let count = leftData.value.filter((v) => !v.disable).length;
  // 方法一
  count == leftData.value.filter((v) => v.check).length ? checkAll.value = true : checkAll.value = false;
  // 方法二
  // checkAll.value = leftData.value.every((v) => v.check);
}
// 子选项改变全选状态（右）
function checkChange1() {
  // 子选项选中的个数
  let count = rightData.value.filter((v) => v.check).length;
  // 方法一
  // count == leftData.value.length ? checkAll.value = true : checkAll.value = false;
  // 方法二
  // every() 检测数值元素的每个元素是否都符合条件 符合返回 true
  checkAll1.value = rightData.value.every((v) => v.check);
}
</script>

<style scoped>
@import "@/CSS/index.css";
</style>
