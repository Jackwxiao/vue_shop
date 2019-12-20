<template>
  <div class="loginWrapper">
    <div class="loginBox">
      <div class="avaterBox">
        <img src="../assets/logo.png" alt="picture" />
      </div>
      <el-form :model="formLogin" label-width="0" class="loginForm">
        <el-form-item>
          <el-input v-model="formLogin.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            type="password"
            v-model="formLogin.password"
            prefix-icon="iconfont icon-password"
          ></el-input>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary">登录</el-button>
          <el-button type="info">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      formLogin: {
        username: 'asad',
        password: '1234445'
      }
    }
  }
}
</script>

<style lang="less" scoped>
.loginWrapper {
  height: 100%;
  background-color: cadetblue;
}
.loginBox {
  position: absolute;
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 0 10px 0 rgba(0, 0, 2, 0.5);
  .avaterBox {
    width: 135px;
    height: 135px;
    border: 1px solid grey;
    border-radius: 50%;
    padding: 10px;
    background-color: #fff;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 10px 0 rgba(0, 0, 2, 0.5);
    img {
      width: 100%;
      height: 100%;
      border: 1px solid grey;
      background-color: #eee;
      border-radius: 50%;
      box-shadow: 0 0 10px 0 rgba(0, 0, 2, 0.5);
    }
  }
}
.loginForm {
  position: absolute;
  bottom: 0;
  padding: 0 20px;
  width: 100%;
  .btns {
    display: flex;
    justify-content: flex-end;
  }
}
</style>
