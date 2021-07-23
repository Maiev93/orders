<template>
  <div id="app">
    <div>
      <button @click="download()">Загрузить данные</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      requesttoken: "",
    };
  },
  methods: {
    download: function () {
      const publicKey = "38cd79b5f2b2486d86f562e3c43034f8";
      const privateKey = "8e49ff607b1f46e1a5e8f6ad5d312a80";
      this.request();
      setTimeout(
        () => this.access(this.requesttoken, publicKey, privateKey),
        1000
      );
    },
    request: function () {
      fetch("http://api.pixlpark.com/oauth/requesttoken", {
        mode: "no-cors",
      }).then((response) => {
        console.log(response);
        return (this.requesttoken = response);
      });
    },
    access: function () {
      return fetch("http://api.pixlpark.com/oauth/accesstoken?oauth_token=318450f77fb845c3b80f453e63569f04&grant_type=api&username=38cd79b5f2b2486d86f562e3c43034f8&password=318450f77fb845c3b80f453e63569f048e49ff607b1f46e1a5e8f6ad5d312a80", {
        mode: "no-cors",
        credentials: "include",
      }).then((res) => {
        console.log(res);
      });
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
