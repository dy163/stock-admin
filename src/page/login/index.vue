<template>
  <div class="login-wrap" :style="back">
    <div class="login-content">
      <el-form ref="form" :model="form" label-width="60px">
        <el-form-item label="账户">
          <el-input v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="form.password" type="password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <el-form-item class="login-btn">
          <el-button type="primary" @click="handleLogin">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { userLogin } from "@/api/user";
import { saveUser, saveUserName } from "@/utils/auth";

const initCodeTimeSeconds = 60;
export default {
  name: "LoginIndex",
  data() {
    return {
      form: {
        username: "admin",
        password: "admin",
        agree: ""
      },
      codeTimer: null, // 倒计时定时器
      codeTimeSeconds: initCodeTimeSeconds, // 定时器事件
      loadingLogin: false,
      codeLoading: false,
      back: {
        backgroundImage: "url(" + require("@/assets/back-img/timg.jpg") + ")",
        backgroundRepeat: "no-repeat",
        backgroundSize: "100% 100%"
      }
    };
  },
  created() {},
  methods: {
    // 登录
    async handleLogin() {
      try {
        const formData = new FormData();
        formData.append("username", this.form.username);
        formData.append("password", this.form.password);
        const res = await userLogin(formData);
        const userInfo = res.data.result.sessionid;
        const username = this.form.username;
        saveUser(userInfo);
        saveUserName(username);
        this.$message({
          message: "恭喜你，登录成功",
          type: "success"
        });
        this.$router.push("/");
      } catch (err) {
        this.$message({
          message: "警告哦，登录失败",
          type: "warning"
        });
      }
    }
  }
};
</script>

<style lang="less" scoped>
.login-wrap {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  // .el-form,
  // .form-content {
  //   // padding: 30px 15px;
  //   border-radius: 10px;
  //   border: 1px solid #000;
  //   .el-form-item {
  //     display: flex;
  //     justify-content: center;
  //   }
  //   /deep/.el-form-item__content {
  //     width: 300px;
  //   }
  //   .login-agree {
  //     width: 100%;
  //     text-align: center;
  //     margin-bottom: 10px;
  //   }
  //   .btn-login {
  //     margin: 0;
  //     .el-button {
  //       width: 100%;
  //     }
  //   }
  // }
}
.login-content {
  width: 500px;
  border: 1px solid #000;
  border-radius: 10px;
  padding: 30px 20px 30px 0;
  .el-form-item {
    /deep/.el-form-item__label {
      color: #000;
    }
  }
  .login-btn {
    margin: 0;
    .el-button {
      width: 100%;
    }
  }
}
</style>>
