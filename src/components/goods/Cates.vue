<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>商品管理</el-breadcrumb-item>
      <el-breadcrumb-item>商品列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
      <el-row>
        <el-button type="primary">添加分类</el-button>
      </el-row>
    </el-card>
  </div>
</template>
<script>
export default {
  data() {
    return {
        querInfo: {
           type: 3,
           pagenum: 1,
           pagesize: 5
        },
        cateList: [],
        total: 0
    }
  },
  created() {
      // 获取商品分裂数据
      this.getCateList()
  },
  methods: {
     async getCateList() {
        const { data: res } = await this.$http.get('categories', {
            params: this.querInfo })
            if (res.meta.status !== 200) {
                return this.$message.error('获取商品列表失败！')
            }
            console.log(res.data)
            this.cateList = res.data.result// 数据列表
            this.total = res.data.total// 总数据条数
  }
}
}
</script>
<style lang="less" scoped>
</style>
