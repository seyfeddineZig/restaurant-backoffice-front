<template>
  <div class>
    <div class="row window-height window-width row justify-center items-center">
      <div class="col-md-3">
        <LoginForm></LoginForm>
      </div>
    </div>
  </div>
</template>

<script>
import LoginForm from "@/components/LoginForm.vue";
export default {
  name: "Login",
  components: { LoginForm }
};
</script>

<style>
</style>
