<template>
  <div class="reg">
    <!-- 顶部 -->
    <reg-header text="注册bilibili">
      <div slot="right" @click="$router.push('/login')">切换登录</div>
    </reg-header>

    <!-- 注册输入框 中文验证：^[\u4E00-\u9FA5\uf900-\ufa2d·s]{2,20}$-->
    <reg-input
      style="marginBottom:15px"
      label="姓名"
      type="username"
      placeholder="请输入姓名"
      rule="^[a-zA-Z\u4e00-\u9fa5]+$"
      @regvalue="user => this.name = user"
      @Color="user => this.Color_name = user"
    />

    <reg-input
      label="账号"
      type="username"
      placeholder="请输入用户名"
      rule="^[a-zA-Z0-9]{4,8}$"
      @regvalue="user => this.username = user"
      @Color="username => this.Color_username = username"
    />

    <reg-input
      label="密码"
      type="passwor"
      placeholder="6~16个字符，区分大小写"
      rule="[0-9a-zA-Z]{6,19}"
      @regvalue="user => this.password = user"
      @Color="password => this.Color_password = password"
    />

    <!-- 注册按钮 -->
    <reg-botton :color="color" text="注册" @regBtn="regBtn" />

    <div class="login">
      注册即代表你同意
      <a href="javascript:;">用户协议</a>和
      <a href="javascript:;">隐私政策</a>
    </div>
  </div>
</template>

<script>
import RegHeader from "components/common/RegHeader";
import RegInput from "components/common/RegInput";
import RegBotton from "components/common/RegBotton";

export default {
  name: "Register",
  data() {
    return {
      name: "",
      username: "",
      password: "",
      color: "#ff9db5",

      // 实时获取信息
      Color_name: "",
      Color_username: "",
      Color_password: ""
    };
  },
  components: {
    RegHeader,
    RegInput,
    RegBotton
  },
  computed: {
    ColorLength() {
      let { Color_name, Color_username, Color_password } = this;
      return {
        Color_name,
        Color_username,
        Color_password
      };
    }
  },
  watch: {
    ColorLength(val) {
      if (val.Color_name && val.Color_username && val.Color_password) {
        this.color = "#fb7299";
      }
    }
  },
  methods: {
    // 注册请求
    async regBtn() {
      if (this.name && this.username && this.password) {
        let res = await this.$http.post("/register", {
          username: this.username,
          password: this.password,
          name: this.name
        });

        if (res.data.code == 200) {
          localStorage.setItem('token',res.data.objtoken);
          localStorage.setItem('id',res.data.id);
          this.$Toast(res.data.msg);
          setTimeout(() => {
            this.$router.push("/userinfo");
          }, 1000);
        } else {
          console.log(res);
          this.$Toast.fail(res.data.msg);
        }
      } else {
        this.$Toast.fail("请输入信息");
      }
    }
  },
  created() {},
  mounted() {}
};
</script>
<style scoped lang="scss">
.reg {
  height: 100%;
  background-color: rgb(244, 244, 244);
}
.login {
  margin-top: 50px;
  text-align: center;
  font-size: 10px;
  color: #757575;
  a {
    color: #fb7299;
  }
}
</style>
