<!--HTML代码,显示数据-->
<template>
  <div class="schooltable">
    <el-table :data="data" border style="width: 100%">
      <el-table-column prop="id" label="ID" width="100" />
      <el-table-column prop="name" label="名称" />
    </el-table>
  </div>
</template>

<script setup>
// 导入模块
import axios from 'axios'
import { ref } from 'vue';
import { onMounted } from 'vue';
// 新建空表
const data = ref([])

// 访问url并获取数据
onMounted(() => {
  axios.get('http://localhost:5106/SchoolTable/select')
    .then((res) => {
      data.value = res.data
    })
})
</script>

<style>
@media (min-width: 1024px) {
  .schooltable {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
