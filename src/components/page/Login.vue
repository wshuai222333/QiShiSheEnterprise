<template>
  <div class="login-wrap">
    <div class="ms-login">
      <div class="ms-title">后台管理系统</div>
      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="ms-content">
        <el-form-item prop="username">
          <el-input v-model="ruleForm.username" placeholder="手机号">
            <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            type="password"
            placeholder="密码"
            v-model="ruleForm.password"
            @keyup.enter.native="submitForm('ruleForm')"
          >
            <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
          </el-input>
        </el-form-item>
        <div class="login-btn">
          <el-button type="primary" @click="submitForm('ruleForm')" :loading="isLoging">登录</el-button>
        </div>
        <p class="login-tips"></p>
      </el-form>
    </div>
  </div>
</template>

<script>
import Service from "../../_common";

export default {
  data: function() {
    return {
      ruleForm: {
        username: "",
        password: "",
        isLoging: false
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.handleLogin();
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    handleLogin() {
      this.isLoging = true;
      debugger;
      this.$http
        .post(
          "/api/Enterprise/StaffLogin",
          Service.Encrypt.DataEncryption({
            UserName: this.ruleForm.username,
            UserPwd: this.ruleForm.password
          })
        )
        .then(
          response => {
            if (
              response.data.Data &&
              response.data.Data != null &&
              response.data.Data != undefined
            ) {
              if (response.data.Status == 100) {
                response.data.Data.UserPwd = null;

                this.$message.success("登录成功");
                sessionStorage.setItem(
                  "ms_username",
                  JSON.stringify(response.data.Data)
                );
                // Service.Util.SetLocalStorage(Service.Enum.CGT_ALI_USER, "");
                // Service.Util.SetLocalStorage(
                //   Service.Enum.CGT_ALI_USER,
                //   JSON.stringify(response.data.Data)
                // );
                this.$router.push("/");
              } else {
                this.$message(response.data.Message);
              }
            } else {
              this.$message(response.data.Message);
            }
          },
          error => {
            this.$message("请求失败！");
            console.log(error);
          }
        );
      this.isLoging = false;
    }
  }
};
</script>

<style scoped>
.login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background-image: url(../../assets/login-bg.jpg);
  background-size: 100%;
}
.ms-title {
  width: 100%;
  line-height: 50px;
  text-align: center;
  font-size: 20px;
  color: #fff;
  border-bottom: 1px solid #ddd;
}
.ms-login {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 350px;
  margin: -190px 0 0 -175px;
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.3);
  overflow: hidden;
}
.ms-content {
  padding: 30px 30px;
}
.login-btn {
  text-align: center;
}
.login-btn button {
  width: 100%;
  height: 36px;
  margin-bottom: 10px;
}
.login-tips {
  font-size: 12px;
  line-height: 30px;
  color: #fff;
}
</style>