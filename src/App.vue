<template>
  <div>
    <router-view v-if="!loggedIn"></router-view>
    <Layout v-else></Layout>
  </div>
</template>

<script>
import Layout from "./views/Layout.vue";

export default {
  name: "LayoutDefault",

  components: {
    Layout
  },

  data() {
    return {};
  },
  created() {},
  computed: {
    loggedIn() {
      if (localStorage.getItem("token") != null) {
        return true;
      } else {
        return false;
      }
    }
  }
};
</script>

<style lang="scss">
* {
  font-family: "Montserrat", sans-serif;
}
.q-field--outlined .q-field__control:before {
  background: #f2f2f2;
}

.fas.fa-exclamation-circle.q-icon.text-negative[aria-hidden] {
  display: none;
}
</style>
