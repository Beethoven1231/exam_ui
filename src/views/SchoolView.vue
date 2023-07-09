<!--HTML代码,显示数据-->
<template>
  <div class="schooltable">
    <el-button size="small" type="success" @click="handleInsert()">新增</el-button>
    <!--显示学校表数据-->
    <el-table :data="data" border style="width: 100%">
      <el-table-column prop="id" label="ID" width="100" />
      <el-table-column prop="name" label="名称" />
      <!--显示两个按钮-->
      <el-table-column label="操作" width="180">
        <template #default="scope">
          <el-button size="small" @click="handleEdit(scope.row, form)">修改</el-button>
          <el-button size="small" type="danger" @click="handleDelete(scope.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!--显示修改弹窗-->
    <el-dialog v-model="dialogFormVisible" title="修改">
      <el-form :model="form">
        <el-form-item label="学校名称" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off" />
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogFormVisible = false">返回</el-button>
          <el-button type="primary" @click="update()">确认</el-button>
        </span>
      </template>
    </el-dialog>
    <!--显示新增弹窗-->
    <el-dialog v-model="dialogFormVisible2" title="新增">
      <el-form :model="form">
        <el-form-item label="学校名称" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off" />
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogFormVisible2 = false">返回</el-button>
          <el-button type="primary" @click="insert()">确认</el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
// 导入模块
import axios from 'axios'
import { ref } from 'vue';
import { onMounted } from 'vue';
import { ElMessage, ElMessageBox } from 'element-plus'
import { reactive } from 'vue'

// 新建两个变量，控制弹窗显示
const dialogFormVisible = ref(false)
const dialogFormVisible2 = ref(false)
// 文字样式
const formLabelWidth = '140px'
// 输入格式
const form = reactive({
  id: 0,
  name: '',
})
// 新建空表
const data = ref([])

// 访问url并获取数据
onMounted(() => {
  axios.get('http://localhost:5106/SchoolTable/select')
    .then((res) => {
      data.value = res.data
    })
})

const init = () => {
  axios.get('http://localhost:5106/SchoolTable/select')
    .then((res) => {
      data.value = res.data
    })
}
// 修改按钮按下
const handleEdit = (row, form) => {
  form.id = row.id
  form.name = row.name
  dialogFormVisible.value = true
}

// 删除按钮按下
const handleDelete = (id) => {
  ElMessageBox.confirm(
    '删除后数据无法恢复，请谨慎操作',
    '确认删除吗',
    {
      confirmButtonText: '确认',
      cancelButtonText: '返回',
      type: 'warning',
    }
  )
    .then(() => {
      axios.get('http://localhost:5106/SchoolTable/delete?id=' + id)
        .then((res) => {
          init();
        })
      ElMessage({
        type: 'success',
        message: 'Delete completed',
      })
    })
    .catch(() => {
      ElMessage({
        type: 'info',
        message: 'Delete canceled',
      })
    })
}

// 修改弹窗的确认按钮按下
const update = () => {
  axios.get("http://localhost:5106/SchoolTable/update?id=" + form.id + "&name=" + form.name)
  .then(() => {
    dialogFormVisible.value = false
    init();
  })
}

// 新增按钮按下
const handleInsert = () => {
  form.id = 0
  form.name = ''
  dialogFormVisible2.value = true
}

// 新增弹窗的确认按钮按下
const insert = () => {
  axios.get("http://localhost:5106/SchoolTable/insert?name="+ form.name)
  .then(() => {
    dialogFormVisible2.value = false
    init();
  })
}
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
