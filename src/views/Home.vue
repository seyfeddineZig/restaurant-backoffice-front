<template>
  <div class>
    <div class="row window-height window-width row justify-center items-center">
      <div>
        <div class="q-gutter-xl">
          <q-btn push color="white" text-color="primary" label="Caisse" :to="{name: 'login'}" />
          <q-btn push color="blue" text-color="white" label="Paramètres" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",

  components: {},

  data() {
    return {};
  }
};
</script>

<style>
</style>
