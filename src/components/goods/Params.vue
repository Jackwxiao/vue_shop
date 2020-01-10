<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>商品管理</el-breadcrumb-item>
      <el-breadcrumb-item>参数列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
      <el-alert title="注意：" type="warning" description="仅允许修改第三级分类的相关参数！" show-icon></el-alert>
      <el-row class="cate-row">
        <el-col>
          <span class="span">选择商品分类:</span>
          <el-cascader
            expand-trigger="hover"
            v-model="selectedParamsKeys"
            :options="paramsList"
            :props="cascaderProps"
            @change="handleChange"
            clearable
          ></el-cascader>
        </el-col>
      </el-row>
      <el-tabs v-model="activeName" @tab-click="handleTabClick">
        <el-tab-pane label="动态参数" name="many">
          <el-button type="primary" size="mini" :disabled="isBtnDisabled" @click="dialogVisible = true">添加参数</el-button>
          <!-- 动态参数表格 -->
          <el-table :data="manyTableData" border stripe>
            <el-table-column type="expand"></el-table-column>
            <el-table-column type="index"></el-table-column>
            <el-table-column label="参数名称" prop="attr_name"></el-table-column>
            <el-table-column label="操作">
              <template>
                <el-button size="mini" type="primary" icon="el-icon-edit">编辑</el-button>
                <el-button size="mini" type="danger" icon="el-icon-delete">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-tab-pane>
        <el-tab-pane label="静态属性" name="only">
          <el-button type="primary" size="mini" :disabled="isBtnDisabled" @click="dialogVisible = true">添加属性</el-button>
          <!-- 静态属性表格 -->
          <el-table :data="onlyTableData" border stripe>
            <el-table-column type="expand"></el-table-column>
            <el-table-column type="index"></el-table-column>
            <el-table-column label="属性名称" prop="attr_name"></el-table-column>
            <el-table-column label="操作">
              <template>
                <el-button size="mini" type="primary" icon="el-icon-edit">编辑</el-button>
                <el-button size="mini" type="danger" icon="el-icon-delete">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-tab-pane>
      </el-tabs>
    </el-card>
    <!-- 添加参数对话框 -->
    <el-dialog :title="'添加' + titleText" :visible.sync="dialogVisible" width="50%" @close="closeResetDialog">
      <el-form
        :model="addParamsForm"
        :rules="addParamsFormRules"
        ref="addParamsFormRef"
        label-width="80px"
      >
        <el-form-item :label="titleText" prop="attr_name">
          <el-input v-model="addParamsForm.attr_name"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addParams">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      paramsList: [],
      cascaderProps: {
        value: 'cat_id',
        label: 'cat_name',
        children: 'children'
      },
      selectedParamsKeys: [],
      activeName: 'many',
      // 保存动态参数的数据
      manyTableData: [],
      // 保存静态属性的数据
      onlyTableData: [],
      dialogVisible: false,
      // 添加参数/属性的表单数据
      addParamsForm: {
          attr_name: ''
      },
      addParamsFormRules: {
          attr_name: [
               { required: true, message: '请输入参数名', trigger: 'blur' }
          ]
      }
    }
  },
  created() {
    this.getParamsList()
  },
  methods: {
    async getParamsList() {
      const { data: res } = await this.$http.get('categories')
      if (res.meta.status !== 200) {
        return this.$message.error('获取参数分类失败！')
      }
      this.paramsList = res.data
      console.log(this.paramsList)
    },
    // 级联选择框种选项发生变化会触发此函数
    handleChange() {
      this.getParamsData()
    },
    // tab标签点击事件console.log()
    handleTabClick() {
      this.getParamsData()
    },
    async getParamsData() {
      // 选中三级分类
      if (this.selectedParamsKeys.length !== 3) {
        this.selectedParamsKeys = []
      }
      // 根据所选的id 获取对应的参数
      const { data: res } = await this.$http.get(
        `categories/${this.cateId}/attributes`,
        {
          params: { sel: this.activeName }
        }
      )
      if (res.meta.status !== 200) {
        return this.$message.error('获取参数列表失败！')
      }
      console.log(res.data)
      if (this.activeName === 'many') {
        this.manyTableData = res.data
      } else {
        this.onlyTableData = res.data
      }
    },
    // 监听对话框的关闭事件
    closeResetDialog(){
        this.$refs.addParamsFormRef.resetFields()
    },
    addParams() {
        this.$refs.addParamsFormRef.validate(async valid => {
            if (!valid) return
            const { data: res } = await this.$http.post(`categories/${this.cateId}/attributes`, {
                attr_name: this.addParamsForm.attr_name,
                attr_sel: this.activeName
            })
            if (res.meta.status !== 201) {
                return this.$message.error('添加参数失败！')
            }
            this.$message.success('添加参数成功！')
            this.dialogVisible = false
            this.getParamsData()
        })
    }
  },
  computed: {
    // 需要按钮被禁用则返回true
    isBtnDisabled() {
      if (this.selectedParamsKeys.length !== 3) {
        return true
      }
      return false
    },
    cateId() {
      if (this.selectedParamsKeys.length === 3) {
        return this.selectedParamsKeys[2]
      }
      return null
    },
    titleText() {
        if (this.activeName === 'many') {
            return '动态参数'
        }
        return '静态属性'
    }
  }
}
</script>
<style lang="less" scoped>
.cate-row {
  margin-top: 15px;
}
.span {
  margin-right: 1em;
}
</style>
