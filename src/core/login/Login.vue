<template>
  <farm-main :paddingTop="['xl', 'xxl']" :paddingX="['m', 'xl', 'xxl']">
    <select v-model="farmosUrl">
      <option disabled value="">Choose a server</option>
      <option>peritusag.farmos.net</option>
      <option>farmos2.peritusag.com</option>
    </select>
    <br />
    <br />

    <div class="input-group">
      <div class="input-group-prepend">
        <span class="input-group-text">https://</span>
      </div>
      <input
        v-model="farmosUrl"
        :placeholder="$t('Enter your farmOS URL')"
        autofocus
        type="url"
        autocomplete="url"
        class="form-control"
        v-on:input="checkValues"
      />
    </div>

    <br />
    <div class="input-group" id="app">
      <input
        v-model="username"
        :placeholder="$t('Enter your username')"
        type="text"
        autocomplete="username"
        class="form-control"
        v-on:input="checkValues"
      />
    </div>
    <br />
    <div class="input-group">
      <input
        v-model="password"
        :placeholder="$t('Enter your password')"
        type="password"
        autocomplete="current-password"
        class="form-control"
        v-on:input="checkValues"
      />
    </div>
    <br />
    <div class="input-group login-submit">
      <div v-if="authPending">
        <icon-spinner />
      </div>
      <button
        v-else
        :disabled="!this.valuesEntered"
        title="Submit credentials"
        @click="submitCredentials"
        class="btn btn-success btn-navbar"
        type="button"
      >
        {{ $t("Submit credentials") }}
      </button>
    </div>
    <br />
    <p style="textalign: center">
      {{ $t("Need a server? Check out") }}
      <a href="https://farmos.org/hosting/">{{ $t("hosting options") }}</a
      >.
    </p>

    <!-- ======================== -->
    <!-- ======================== -->
    <!-- ======================== -->

    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
    <div id="app">
      储存账号:<input v-model="name" />
      <br />
      储存密码:
      <input type="password" v-model="pw" />
      <p />
      <button @click="persist">保存登陆信息</button>
      <button @click="persist2">删除缓存</button>
    </div>
    <!-- ======================== -->
    <!-- ======================== -->
    <!-- ======================== -->
  </farm-main>
</template>

<script>
export default {
  name: "Login",
  localStorage: {
    str: "",
    num: "",
    name: "",
    pw: "",
  },
  data() {
    if (localStorage.name) {
      console.log("here is username : " + localStorage.name);
      return {
        valuesEntered: false,
        authPending: false,
        username: localStorage.name,
        // password: localStorage.password,
        farmosUrl: "",
        password: localStorage.pw,
        newKeyValueObj: {
          key: "",
          value: "",
        },
        newValueToArr: "",
      };
    } else {
      // console.log("localStorage.name: " + localStorage.name);
      return {
        valuesEntered: false,
        authPending: false,
        username: "",
        password: "",
        farmosUrl: "",

        newKeyValueObj: {
          key: "",
          value: "",
        },
        newValueToArr: "",
      };
    }

    // };
  },
  mounted() {
    if (localStorage.name) this.name = localStorage.name;
    if (localStorage.pw) this.pw = localStorage.pw;
    console.log("start mounted()  now");
    console.log("this.name: " + this.name);
    console.log("this.pw: " + this.pw);
    console.log("end  mounted() ");
    console.log();
    console.log();
  },

  methods: {
    // =====================
    // =====================

    persist() {
      localStorage.name = this.name;
      localStorage.pw = this.pw;
      console.log("start  persist() now");
      console.log("this.name: " + this.name);
      console.log("this.pw: " + this.pw);
      console.log("end  persist()");
      console.log();
      console.log();
    },

    persist2() {
      localStorage.name = "";
      localStorage.pw = "";
    },
    checkValues() {
      const urlIsValid =
        process.env.NODE_ENV === "development" || this.username !== "";
      const usernameIsValid = this.username !== "";
      const passwordIsValid = this.password !== "";
      if (urlIsValid && usernameIsValid && passwordIsValid) {
        this.valuesEntered = true;
      }
    },

    submitCredentials() {
      const payload = {
        farmosUrl: this.farmosUrl,
        username: this.username,
        password: this.password,
        router: this.$router,
      };

      this.authPending = true;

      this.$store
        .dispatch("authorize", payload)
        .then(() => {
          this.authPending = false;
          this.$store.commit("setLoginStatus", true);
          return this.$store.dispatch("updateFieldModules", this.$router);
        })
        .then((res) => this.$store.dispatch("updateUserAndSiteInfo", res))
        .then((res) => this.$store.dispatch("updateFarmResources", res));
    },
  },
  created() {
    this.farmosUrl =
      localStorage.getItem("host")?.replace(/(^\w+:|^)\/\//, "") || "";
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login-submit {
  justify-content: center;
}
</style>
