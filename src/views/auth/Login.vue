<template>
  <b-container class="my-5">
    <b-row align-h="center" style="margin-top:15%">
      <b-form>
        <b-form-group label="用户名:" label-for="UserName">
          <b-form-input id="UserName" v-model="form.UserName" required placeholder="输入用户名"></b-form-input>
        </b-form-group>

        <b-form-group label="密码:" label-for="Password">
          <b-form-input
            type="password"
            id="Password"
            v-model="form.Password"
            required
            placeholder="输入密码"
          ></b-form-input>
        </b-form-group>

        <b-button to="/" variant="info">返回</b-button>&nbsp;
        <b-button @click="Login" variant="danger">登录</b-button>
      </b-form>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      form: {
        UserName: "",
        Password: ""
      }
    };
  },
  methods: {
    Login() {
      if (this.form.UserName != "" && this.form.Password != "") {
        this.axios
          .post(this.$store.state.BASEURL + "/login", this.form)
          .then(response => {
            if (response.data.code == 204) {
              alert(response.data.message);
            } else {
              this.$store.commit("ChangeLoginState", {
                IsLogin: true,
                UserName: this.form.UserName,
                Token: response.data.Token,
                UserID: response.data.UserID
              });
              this.$router.push("/home");
            }
          });
      } else {
        this.$bvToast.toast("用户名或者密码不能为空!", {
          title: "消息",
          variant: "danger",
          solid: true
        });
      }
    }
  },
  mounted() {
    this.$parent.show_nav = false;
  },
  destroyed() {
    this.$parent.show_nav = true;
  }
};
</script>

<style scoped>
</style>