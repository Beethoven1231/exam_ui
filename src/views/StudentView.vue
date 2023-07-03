<!--HTML代码,显示数据-->
<template>
  <div class="studenttable">
    <el-button size="small" type="success" @click="handleInsert()">新增</el-button>
    <!--显示学生表-->
    <el-table :data="data" border style="width: 100%">
      <el-table-column prop="id" label="ID" width="100" />
      <el-table-column prop="name" label="名称" />
      <el-table-column prop="birthday" label="生日" />
      <el-table-column prop="classId" label="所属班级" />
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
        <el-form-item label="名称" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="生日" :label-width="formLabelWidth">
          <el-input v-model="form.birthday" autocomplete="off" />
        </el-form-item>
        <el-form-item label="班级编号" :label-width="formLabelWidth">
          <el-input v-model="form.classId" autocomplete="off" />
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
        <el-form-item label="名称" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="生日" :label-width="formLabelWidth">
          <el-input v-model="form.birthday" autocomplete="off" />
        </el-form-item>
        <el-form-item label="班级编号" :label-width="formLabelWidth">
          <el-input v-model="form.classId" autocomplete="off" />
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

<script lang="ts" setup>
// 导入模块
import axios from 'axios'
import { ref } from 'vue';
import { onMounted } from 'vue';
import { ElMessage, ElMessageBox } from 'element-plus'
import { reactive } from 'vue'

const dialogFormVisible = ref(false)
const dialogFormVisible2 = ref(false)
const formLabelWidth = '140px'
const form = reactive({
  id: 0,
  name: '',
  birthday: '',
  classId: '',
})
// 新建空表
const data = ref([])

// 访问url并获取数据
onMounted(() => {
  init();
})

const init = () => {
  axios.get('http://localhost:5106/StudentTable/select')
    .then((res) => {
      data.value = res.data
    })
}

const handleEdit = (row, form) => {
  form.id = row.id
  form.name = row.name
  form.birthday = row.birthday
  form.classId = row.classId
  dialogFormVisible.value = true
}

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
      axios.get('http://localhost:5106/StudentTable/delete?id=' + id)
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

const update = () => {
  axios.get("http://localhost:5106/StudentTable/update?id=" + form.id + "&name=" + form.name + "&birthday=" + form.birthday + "&classId=" + form.classId)
  .then(() => {
    dialogFormVisible.value = false
    init();
  })
}

const handleInsert = () => {
  form.id = 0
  form.name = ''
  form.birthday = ''
  form.classId = ''
  dialogFormVisible2.value = true
}

const insert = () => {
  axios.get("http://localhost:5106/StudentTable/insert?name="+ form.name +"&birthday="+ form.birthday +"&classId=" + form.classId)
  .then(() => {
    dialogFormVisible2.value = false
    init();
  })
}
</script>


<style scoped>
@media (min-width: 1024px) {
  .studenttable {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }

  .el-button--text {
    margin-right: 15px;
  }

  .el-select {
    width: 300px;
  }

  .el-input {
    width: 300px;
  }

  .dialog-footer button:first-child {
    margin-right: 10px;
  }
}
</style>
