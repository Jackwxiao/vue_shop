<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>角色列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
      <el-row>
        <el-col>
          <el-button type="primary" @click="addDialog">添加角色</el-button>
        </el-col>
      </el-row>
      <el-table :data="rolesList" border stripe>
        <el-table-column type="expand">
          <template slot-scope="scope">
            <el-row
              :class="['borderBottom', i1  === 0 ? 'borderTop' : '', 'vcenter']"
              v-for="(item1, i1) in scope.row.children"
              :key="item1.id"
            >
              <el-col :span="5">
                <el-tag type="success">{{item1.authName}}</el-tag>
                <i class="el-icon-caret-right"></i>
              </el-col>
              <el-col :span="19">
                <el-row
                  :class="[i2 === 0 ? '': 'borderTop', 'vcenter']"
                  v-for="(item2, i2) in item1.children"
                  :key="item2.id"
                >
                  <el-col :span="6">
                    <el-tag>{{item2.authName}}</el-tag>
                    <i class="el-icon-caret-right"></i>
                  </el-col>
                  <el-col :span="18">
                      <el-tag type="warning" v-for="(item3) in item2.children" :key="item3.id">
                          {{item3.authName}}
                      </el-tag>
                  </el-col>
                </el-row>
              </el-col>
            </el-row>
          </template>
        </el-table-column>
        <el-table-column type="index"></el-table-column>
        <el-table-column label="角色名称" prop="roleName"></el-table-column>
        <el-table-column label="角色描述" prop="roleDesc"></el-table-column>
        <el-table-column label="操作" width="300px">
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="primary"
              icon="el-icon-edit"
              @click="expandEditDialog(scope.row.id)"
            >编辑</el-button>
            <el-button
              size="mini"
              type="danger"
              icon="el-icon-delete"
              @click="removeRoleByRoleId(scope.row.id)"
            >删除</el-button>
            <el-button size="mini" type="warning" icon="el-icon-setting">分配角色</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
    <!-- 添加角色对话框 -->
    <el-dialog title="添加角色" :visible.sync="dialogVisible" width="50%" @close="closeResetDialog">
      <el-form
        :model="addRolesForm"
        :rules="addRolesFormRules"
        ref="addRolesFormRef"
        label-width="80px"
      >
        <el-form-item label="角色名称" prop="roleName">
          <el-input v-model="addRolesForm.roleName"></el-input>
        </el-form-item>
        <el-form-item label="角色描述" prop="roleDesc">
          <el-input v-model="addRolesForm.roleDesc"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addRoles">确 定</el-button>
      </span>
    </el-dialog>
    <!-- 修改角色对话框 -->
    <el-dialog
      title="修改角色"
      :visible.sync="editRolesDialogVisible"
      width="50%"
      @close="cancelResetDialog"
    >
      <el-form
        :model="editRolesForm"
        :rules="addRolesFormRules"
        ref="editRolesFormRef"
        label-width="80px"
      >
        <el-form-item label="角色名称" prop="roleName">
          <el-input v-model="editRolesForm.roleName"></el-input>
        </el-form-item>
        <el-form-item label="角色描述" prop="roleDesc">
          <el-input v-model="editRolesForm.roleDesc"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editRolesDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editRolesInfo">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rolesList: [],
      dialogVisible: false,
      addRolesForm: {
        roleName: '',
        roleDesc: ''
      },
      addRolesFormRules: {
        roleName: [
          { required: true, message: '请输入角色名', trigger: 'blur' }
        ],
        roleDesc: [{ required: true, message: '请输入角色名', trigger: 'blur' }]
      },
      editRolesDialogVisible: false,
      editRolesForm: {}
    }
  },
  created() {
    this.getRolesList()
  },
  methods: {
    async getRolesList() {
      const { data: res } = await this.$http.get('roles')
      if (res.meta.status !== 200) {
        return this.$message.error('获取角色列表失败！')
      }
      this.rolesList = res.data
    },
    addDialog() {
      this.dialogVisible = !this.dialogVisible
    },
    closeResetDialog() {
      this.$refs.addRolesFormRef.resetFields()
    },
    addRoles() {
      this.$refs.addRolesFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('roles', this.addRolesForm)
        if (res.meta.status !== 201) {
          this.$message.error('角色添加失败！')
        }
        this.dialogVisible = false
        this.getRolesList()
        this.$message.success('角色添加成功！')
      })
    },
    async expandEditDialog(id) {
      const { data: res } = await this.$http.get('roles/' + id)
      if (res.meta.status !== 200) {
        return this.$message.error('查询角色信息失败')
      }
      this.editRolesForm = res.data
      this.editRolesDialogVisible = true
    },
    cancelResetDialog() {
      this.$refs.editRolesFormRef.resetFields()
    },
    editRolesInfo() {
      this.$refs.editRolesFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.put(
          'roles/' + this.editRolesForm.roleId,
          {
            roleName: this.editRolesForm.roleName,
            roleDesc: this.editRolesForm.roleDesc
          }
        )
        if (res.meta.status !== 200) {
          this.$message.error('更新失败！')
        }
        this.editRolesDialogVisible = false
        this.getRolesList()
        this.$message.success('更新成功！')
      })
    },
    async removeRoleByRoleId(id) {
      const confirmResult = await this.$confirm(
        '将永久删除该用户，是否继续操作？',
        '温馨提示',
        {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }
      ).catch(err => err)
      if (confirmResult !== 'confirm') {
        return this.$message.info('取消删除')
      }
      const { data: res } = await this.$http.delete('roles/' + id)
      if (res.meta.status !== 200) {
        return this.$message.error('删除角色失败！')
      }
      this.$message.success('删除成功！')
      this.getRolesList()
    }
  }
}
</script>

<style lang="less" scoped>
.el-tag {
  margin: 8px;
}
.borderTop {
  border-top: 1px solid #eee;
}
.borderBottom {
  border-bottom: 1px solid #eee;
}
.vcenter{
    display: flex;
    align-items: center;
}
</style>
