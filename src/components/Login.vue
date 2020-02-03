<template>
    <div class="login_container">
        <div class="login_box">
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="">
            </div>
            <!--登录表单-->
            <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="el-icon-search"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input type="password" v-model="loginForm.password" prefix-icon="el-icon-search"></el-input>
                </el-form-item>
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>export default {
  data () {
    return {
      loginForm: {
        username: 'username',
        password: 'password'
      },
      loginFormRules: {
        username: [
          { required: true, message: '用户名不能为空', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在3到10个字符之间' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败!')
        this.message.success('登录成功!')
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>
<!--1.将登录成功后的token,保存到客户端的 sessionStorage中-->
<!--  1.1项目中出了登录之外的其他API接口,必须在登录之后才能访问-->
<!--  1.2token只应该在当前网站打开期间生效,所以将token保存在sessionStorage中-->
<!--2.通过编程式导航跳转到后台主页,路由地址是/home-->
<!--加上scoped样式只在当前组件内生效,否则全局生效,只要是单文件组件就要加上这个-->
<style lang="less" scoped>
    .login_container{
        background-color: #2b4b6b;
        height: 100%;
    }
    .login_box{
        width: 450px;
        height: 300px;
        background-color: #fff;
        border-radius: 3px;
        position: absolute;
        left:50%;
        top: 50%;
        transform: translate(-50%,-50%);

        .avatar_box{
            position: absolute;
            left: 50%;
            transform: translate(-50%,-50%);
            box-shadow: 0 0 100px #dddddd;
            padding: 10px;
            border: 1px solid #eee;
            border-radius: 50%;
            height: 130px;
            width: 130px;
            img{
                height: 100%;
                width: 100%;
                border-radius: 50%;
                background-color: #eeeeee;
            }
        }
    }
    .btns{
      display: flex;
      justify-content: flex-end;
    }
  .login_form{
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
  }
</style>
