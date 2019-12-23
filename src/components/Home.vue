<template>
  <el-container class="homeContainer">
    <el-header>
      <div>
        <img src="../assets/images/home-header.png" alt />
        <span>xx电商管理后台</span>
      </div>
      <el-button type="info" @click="logout">退出登录</el-button>
    </el-header>
    <el-container>
      <el-aside width="200px">
        <el-menu background-color="#1E1E2D" text-color="#fff" active-text-color="#ffd04b">
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>导航一</span>
            </template>
            <el-menu-item index="1-4-1">
              <template slot="title">
                <i class="el-icon-location"></i>
                <span>导航一</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>Main</el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {
    logout() {
      // 清空 token
      window.sessionStorage.clear()
      // 跳转到登录页
      this.$router.push('/login')
    }
  }
}
</script>

<style lang="less" scoped>
.homeContainer {
  height: 100%;
}
.el-header {
  background-color: #101018;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  > div {
    display: flex;
    align-items: center;
    > span {
      color: #fff;
      font-size: 20px;
      margin-left: 10px;
    }
    > img {
      width: 60px;
      height: 60px;
    }
  }
}
.el-aside {
  background-color: #1e1e2d;
}
.el-main {
  background-color: #f2f3f8;
}
</style>
