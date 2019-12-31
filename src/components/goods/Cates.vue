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
    return {}
  },
  created() {},
  methods() {}
}
</script>
<style lang="less" scoped>
</style>
