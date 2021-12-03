<template>
  <Landing v-if="!userIsAuthorized" />
  <div v-else id="q-app">
    <router-view />
  </div>
</template>
<script>
import Landing from "./layouts/Landing.vue";
import { mapGetters } from "vuex";
import Api from "./services/lib";
import { bus } from "./bus";

// Подключаем плагин чтобы не переворачивался экран
document.addEventListener(
  "deviceready",
  () => {
    screen.orientation.lock("portrait");
  },
  false
);

export default {
  name: "App",
  data() {
    return {};
  },
  computed: {
    ...mapGetters(["userIsAuthorized"])
  },
  created() {
    const currentToken = Api.getCookie("token");
    if (currentToken) {
      // Токен в куках есть, надо попробовать отобразхить страницу с ним.
      this.$store.dispatch("setToken", true);
    }
  },
  beforeCreate() {
    //сохраняем хост в vuex
    this.$store.commit("createHostName", window.location.href);
  },
  components: {
    Landing
  }
};
</script>
