<template>
  <div class="login_container">
    <div class="login_box">
        <!-- 头像区域 -->
        <div class="avatar_box">
            <img src="https://dss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=3551319194,841608426&fm=26&gp=0.jpg" alt="">
        </div>
        <!-- 表单区域 -->
        <!-- 通过 rules 属性传入约定的验证规则，并将 Form-Item 的 prop 属性设置为需校验的字段名即可。 -->
        <el-form ref="login_form" :model="loginForm" label-width="80px" :rules="loginFormRules">
            <el-form-item label="账号" prop="username">
                <el-input prefix-icon="iconfont icon-people" v-model="loginForm.username">
                </el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
                <el-input v-model="loginForm.password">
                  <i slot="prefix" class="iconfont icon-password"></i>
                </el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="login()">登录</el-button>
              <el-button type="info" @click="reset()">重置</el-button>
            </el-form-item>
        </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        // 问题：el ui怎样实现清除前后空格（除了原生js方法）
        username: [
          { required: true, message: '请输入账号', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 11, message: '长度在 6 到 11个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 重置表单
    reset () {
      this.$refs.login_form.resetFields()
    },
    login () {
      this.$refs.login_form.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        console.log(res)

        if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
        this.$message.success(res.meta.msg)
        /*
          登录成功后token保存到sessionStorage中
        */
        window.sessionStorage.setItem('token', res.data.token)

        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  background: #2b4b6b;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  .avatar_box{
      width: 200px;
      height: 120px;
      padding: 10px;
      border: 1px solid #eee;
      border-radius: 50%;
      box-shadow: 0 0 10px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%,-50%);
      background: #fff;
      img{
          width: 100%;
          height: 100%;
          border-radius: 50%;
          background-color: #eee;
      }
  }
}
</style>
