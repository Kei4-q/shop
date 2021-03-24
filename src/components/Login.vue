<template>
  <div class="login_container">
    <div class="login_box">
      <div class="abatar_box">
        <img class="img" src="../assets/logo.png" alt="" />
        <el-form
          ref="loginFormRef"
          label-width="50px"
          class="login_form"
          :model="loginForm"
        >
          <el-form-item>
            <el-input label="用户名" v-model="loginForm.username"></el-input>
          </el-form-item>
          <el-form-item>
            <el-input
              label="密码"
              type="password"
              v-model="loginForm.password"
            ></el-input>
          </el-form-item>
          <el-form-item class="btns">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="resetLoginForm">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields()
    },
    login() {
      this.$refs.loginFormRef.validata(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;

  .login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    .abatar_box {
      height: 130px;
      width: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      .img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
      .login_form {
        width: 450px;
        position: absolute;
        top: 170px;
        left: -170px;
        box-sizing: border-box;
      }
      .btns {
        display: flex;
        justify-content: flex-end;
      }
    }
  }
}
</style>
