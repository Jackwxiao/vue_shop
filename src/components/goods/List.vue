<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>商品管理</el-breadcrumb-item>
      <el-breadcrumb-item>商品列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
      <el-row :gutter="20">
        <el-col :span="8">
          <el-input placeholder="请输入内容">
            <el-button slot="append" icon="el-icon-search"></el-button>
          </el-input>
        </el-col>
        <el-col :span="4">
            <el-button type="primary">添加商品</el-button>
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>
<script>
export default {
  data() {
      return {
          // 查询参数列表
          queryInfo: {
              query: '',
              pagenum: 1,
              pagesize: 10
          },
          // 商品列表
          goodslist: [],
          // 商品总数
          total: 0
      }
  },
  created() {
      this.getGoodsList()
  },
  methods: {
     async getGoodsList() {
         const { data: res } = await this.$http.get('goods', {
              params: this.queryInfo
          })
          if (res.meta.status !== 200) {
              return this.$message.error('获取商品列表失败！')
          }
          this.$message.success('获取商品列表成功！')
          console.log(res.data)
          this.goodslist = res.data.goods
          this.total = res.data.total
      }
  }
}
</script>
<style lang="less" scoped>
</style>
