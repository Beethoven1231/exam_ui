<!--HTML代码,显示数据-->
<template>
  <div class="classtable">
    <el-table :data="data" border style="width: 100%">
      <el-table-column prop="id" label="ID" width="100" />
      <el-table-column prop="grade" label="创建时间" />
      <el-table-column prop="classno" label="班级编号" />
      <el-table-column prop="schoolId" label="所属学校" />
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
onMounted(()=>{
  axios.get('http://localhost:5106/ClassTable/select')
    .then((res)=>{
      data.value = res.data
    })
})
</script>

<style>
@media (min-width: 1024px) {
  .classtable {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
