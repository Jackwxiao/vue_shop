<template>
  <el-button type="info" @click="logout">退出登录</el-button>
</template>

<script>
export default {
    data(){
    return {}
  },
  methods: {
    logout () {
      // 清空 token
      window.sessionStorage.clear()
      // 跳转到登录页
      this.$router.push('/login')
    }
  }
}
</script>

<style lang="less" scoped>

</style>
