<template>
  <div class="login-container">
    <el-form
      class="login-form"
      label-width="100px"
      :model="form"
      :rules="rules"
      ref="loginForm"
    >
      <div class="title-container">
        <img class="title-img" src="/gj.jpg" alt="" />
      </div>

      <el-form-item prop="email" label="邮箱">
        <el-input v-model="form.email" placeholder="请输入邮箱"></el-input>
      </el-form-item>
      <el-form-item prop="captcha" label="验证码" class="captcha-container">
        <div class="captcha">
          <img :src="code.captcha" @click="resetCaptcha" alt="" />
        </div>
        <el-input v-model="form.captcha" placeholder="验证码"></el-input>
      </el-form-item>
      <el-form-item prop="password" label="密码">
        <el-input type="password" v-model="form.password" placeholder="请输入密码"></el-input>
      </el-form-item>
      <el-form-item label="">
        <el-button @click.native.prevent="handleLogin" type="primary">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
import md5 from 'md5'
export default {
  layout: "login",
  data() {
    return {
      form: {
        email: "898488376@qq.com",
        password: "gaojing123",
        captcha: ""
      },
      rules: {
        email: [
          {
            required: true,
            message: "请输入正确的邮箱格式"
          }
        ],
        captcha: [
          {
            required: true,
            message: "请输入验证码"
          }
        ],
          password: [
          {
            required: true,
            pattern: /^[\w_-]{6,12}$/g,
            message: "请输入6～12位密码"
          }
        ]
      },
      code: {
        captcha: "/api/captcha"
      }
    };
  },
  methods: {
      resetCaptcha(){
        this.code.captcha = '/api/captcha?_t'+new Date().gettime()
      },
      handleLogin() {
          this.$refs.loginForm.validate(async valid => {
              if(valid){
                  console.log('校验成功')
                  // @todo 发送注册请求
                  let obj = {
                      email: this.form.email,
                      password: md5(this.form.password),
                      captcha: this.form.captcha
                  }
                    let ret = await this.$http.post('/user/login', obj)
                    if(ret.code == 0) {
                      // 存储token
                      this.$message.success('登录成功')
                      setTimeout(()=>{
                        this.$router.push('/')
                      },500)
                    } else {
                        this.$message.error(ret.message)
                    }

              } else {
                  console.log('校验失败')
              }
          })
      }
  },
};
</script>
<style lang="stylus"></style>
