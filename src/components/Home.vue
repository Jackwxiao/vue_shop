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
          <!-- 一级菜单 -->
          <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
            <template slot="title">
              <i :class="iconsObj[item.id]"></i>
              <span>{{item.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item :index="subItem.id + ''" v-for="subItem in item.children" :key="subItem.id">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{subItem.authName}}</span>
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
    return {
      menulist: [],
      iconsObj: {
        '125': 'iconfont icon-user1',
        '103': 'iconfont icon-permission',
        '101': 'iconfont icon-shangpinguanli',
        '102': 'iconfont icon-dingdan',
        '145': 'iconfont icon-shuju'
      }
    }
  },
  created(){
    this.getMenuList()
  },
  methods: {
    logout() {
      // 清空 token
      window.sessionStorage.clear()
      // 跳转到登录页
      this.$router.push('/login')
    },
    // 获取左侧菜单 get返回的是promise，用await修饰一下
    async getMenuList(){
      const { data: res } = await this.$http.get('menus')
      console.log(res)
      if (res.meta.status !== 200) return this.$message.console.error('res.meta.msg')
      this.menulist = res.data
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
.iconfont{
  margin-right: 10px;
}
</style>
