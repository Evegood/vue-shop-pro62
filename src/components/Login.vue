<template>
  <div id="login-container">
    <div id="login-box">
      <div id="logo-box">
        <img src="../assets/img/logo.png">
      </div>
      <!-- rules接收校验规则 -->
      <el-form :rules="loginFormRules" ref="loginFormRef" :model="loginForm">
        <!-- prop校验规则指向 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username">
            <i slot="prefix" class="iconfont icon-user"></i>
          </el-input>
        </el-form-item>
        <!-- prop校验规则指向 -->
        <el-form-item prop="password">
          <el-input type="password" v-model="loginForm.password">
            <i slot="prefix" class="iconfont icon-3702mima"></i>
          </el-input>
        </el-form-item>
        <el-row>
          <!-- 在此设置了偏移量 -->
          <el-col :offset="15">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="resetForm">重置</el-button>
          </el-col>
        </el-row>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 登录form表单需要的数据对象
      loginForm: {
        username: '',
        password: ''
      },
      // 给 各个表单域 定义校验规则
      loginFormRules: {
        username: [
          // required:非空 message:错误提示 trigger:触发校验机制
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      }
    }
  },
  methods: {
    // 用户登录系统
    login() {
      // 对登录的form表单进行整体校验
      // this.$refs.loginFormRef.validate(function(valid){})
      this.$refs.loginFormRef.validate(async valid => {
        // valid => true/false
        if (valid) {
          // 用户信息真实性校验
          // axios呆着用户信息去到后端数据库校验
          const { data: res } = await this.$http.post('/login', this.loginForm)

          // 判断用户名或密码 真实性校验失败
          if (res.meta.status !== 200) {
            return this.$message.error('用户名或密码不存在')
          }

          // 通过浏览器的sessionStorage记录服务器返回的token信息
          window.sessionStorage.setItem('token', res.data.token)

          // 页面重定向到后台首页(/home)
          this.$router.push('/home')
        }
      })
    },
    // 对表单实现重置
    resetForm() {
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>

<style lang="less" scoped>
#login-container {
  background-color: #2b4b6b;
  height: 100%;
  overflow: hidden;
  #login-box {
    width: 450px;
    height: 304px;
    background-color: #fff;
    border-radius: 4px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .el-form {
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 20px;
      box-sizing: border-box;
    }
    #logo-box {
      width: 130px;
      height: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 8px;
      box-shadow: 0 0 10px #eee;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }
}
</style>
