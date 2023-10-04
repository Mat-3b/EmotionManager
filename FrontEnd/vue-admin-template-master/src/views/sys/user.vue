<template>
  <div>
    <el-card id="search">



      <el-row>
        <el-col :span="20">
          <el-input v-model="searchModel.username" placeholder="用户名"></el-input>
          <el-button @click="getUserList" type="primary" round icon="el-icon-search">查询</el-button>
        </el-col>
        <el-col :span="4" align="right">
          <el-button @click="openEditUI" type="primary" icon="el-icon-plus" circle></el-button>
        </el-col>
      </el-row>
    </el-card>


    <el-card>
      <el-table :data="userList" stripe style="width: 100%">
        <el-table-column label="#" width="80">
          <template slot-scope="scope">
            {{ (searchModel.pageNo-1)*searchModel.pageSize + scope.$index +1 }}
          </template>
        </el-table-column>
        <el-table-column prop="username" label="用户名" width="180">
        </el-table-column>
        <el-table-column prop="gid" label="编号" width="180">
        </el-table-column>
        <el-table-column prop="age" label="年龄" width="80">
        </el-table-column>
        <el-table-column prop="email" label="邮箱" width="180">
        </el-table-column>
        <el-table-column prop="sex" label="性别" width="80">
        </el-table-column>
        <el-table-column prop="status" label="用户状态" width="80">
          <template slot-scope="scope">
            <el-tag v-if="scope.row.status == 1">正常</el-tag>
            <el-tag v-else type="danger">禁用</el-tag>
          </template>
        </el-table-column>
        <el-table-column prop="backup" label="备注">
        </el-table-column>
        <el-table-column label="操作" width="180">
        </el-table-column>
      </el-table>
    </el-card>

    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="searchModel.pageNo"
      :page-sizes="[5, 10, 20, 50]"
      :page-size="searchModel.pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>


  <el-dialog @close="clearForm" :title="title" :visible.sync="dialogFormVisible">
    <el-form :model="userForm" ref="userFormRef" :rules="rules">
      <el-form-item label="用户名" prop="username" :label-width="formLabelWidth">
        <el-input v-model="userForm.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="登录密码" prop="password" :label-width="formLabelWidth">
        <el-input type="password" v-model="userForm.password" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="编号" prop="gid" :label-width="formLabelWidth">
        <el-input v-model="userForm.gid" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="年龄" prop="age" :label-width="formLabelWidth">
        <el-input v-model="userForm.age" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="邮箱" prop="email" :label-width="formLabelWidth">
        <el-input v-model="userForm.email" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="性别" prop="sex" :label-width="formLabelWidth">
        <el-input v-model="userForm.sex" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="备注" prop="backup" :label-width="formLabelWidth">
        <el-input v-model="userForm.backup" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="用户状态" :label-width="formLabelWidth">
          <el-switch
            v-model="userForm.status"
            :active-value="1"
            :inactive-value="0"
            >
          </el-switch>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogFormVisible = false">取 消</el-button>
      <el-button type="primary" @click="saveUser">确 定</el-button>
    </div>
  </el-dialog>


  </div>
</template>

<script>
import userApi from '@/api/userManage'
export default {
  data() {
    return {
      formLabelWidth: '130px',
      userForm: {},
      dialogFormVisible: false,
      title: "",
      total: 0,
      searchModel: {
        pageNo: 1,
        pageSize: 10,
      },
      userList: [],
      rules: {
        username: [
            { required: true, message: '请输入用户名', trigger: 'blur' },
            { min: 3, max: 20, message: '长度在 3 到 20 个字符', trigger: 'blur' }
        ],
        password: [
            { required: true, message: '请输入初始密码', trigger: 'blur' },
            { min: 3, max: 20, message: '长度在 3 到 20 个字符', trigger: 'blur' }
        ],
        gid: [
            { required: true, message: '请输入编号', trigger: 'blur' },
            { min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur' }
        ],
        age: [
            { required: true, message: '请输入年龄', trigger: 'blur' },
            { min: 1, max: 4, message: '长度在 1 到 4 个字符', trigger: 'blur' }
        ],
        email: [
            { required: true, message: '请输入邮箱', trigger: 'blur' },
            { min: 3, max: 30, message: '长度在 3 到 30 个字符', trigger: 'blur' }
        ],
        sex: [
            { required: true, message: '请输入性别', trigger: 'blur' },
            { min: 1, max: 5, message: '长度在 1 到 5 个字符', trigger: 'blur' }
        ],
        backup: [
            { required: true, message: '请输入备注', trigger: 'blur' },
            { min: 1, max: 100, message: '长度在 1 到 100 个字符', trigger: 'blur' }
          ],
      },
    }
  },
  methods: {
    saveUser() {
      // 触发表单验证
      this.$refs.userFormRef.validate((valid) => {
          if (valid) {
            // 提交请求
            userApi.addUser(this.userForm).then(response => {
              // 成功提示
              this.$message({
                message: response.message,
                type: 'success'
              });
              // 关闭对话框
              this.dialogFormVisible = false;
              // 刷新数据
              this.getUserList();
            });
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      
    },
    clearForm() {
      this.userForm = {};
      this.$refs.userFormRef.clearValidate();
    },
    openEditUI() {
      this.title = '新增用户';
      this.dialogFormVisible = true;
    },
    handleSizeChange(pageSize) {
      this.searchModel.pageSize = pageSize;
      thie.getUserList();
    },
    handleCurrentChange() {
      this.searchModel.pageNo = pageNo;
      thie.getUserList();
    },
    getUserList() {
      userApi.getUserList(this.searchModel).then(response => {
        this.userList = response.data.rows;
        this.total = response.data.total;
      });
    },
  },
  created() {
    this.getUserList();
  }
}
</script>

<style>
#search .el-input {
  width: 200px;
  margin-right: 10px;
}
.el-dialog .el-input{
  width: 65%;
}
</style>