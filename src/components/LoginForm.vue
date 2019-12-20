<template>
  <q-card class="q-pa-md">
    <q-card-section>
      <div class="text-h6 text-center">S'authentifier</div>
      <br />
      <div class="q-gutter-md">
        <q-form ref="form" @submit="onSubmit" @reset="onReset" class="q-gutter-md">
          <q-input
            class="q-p-l-4"
            v-model="username"
            rounded
            outlined
            type="text"
            hint="Nom d'utilisateur"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Veuillez saisir le nom d\'utilisateur']"
          ></q-input>
          <q-input
            v-model="password"
            rounded
            outlined
            :type="isPwd ? 'password' : 'text'"
            hint="Mot de passe"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Veuillez saisir le mot de passe']"
          >
            <template v-slot:append>
              <q-icon
                :name="isPwd ? 'far fa-eye-slash' : 'far fa-eye'"
                class="cursor-pointer"
                @click="isPwd = !isPwd"
                size="20px"
              />
            </template>
          </q-input>
          <div class="row justify-center q-mt-lg">
            <div class="q-gutter-sm">
              <q-btn push color="white" class="q-pa-sm" rounded text-color="dark" type="reset">
                <q-icon :name="'fas fa-redo-alt'" size="20px" />
              </q-btn>
              <q-btn
                push
                rounded
                class="q-pa-sm"
                color="blue-10"
                text-color="white"
                type="submit"
                label="Valider"
              />
            </div>
          </div>
        </q-form>
      </div>
    </q-card-section>
  </q-card>
</template>

<script>
export default {
  name: "LoginForm",
  data() {
    return {
      password: "",
      username: "",
      isPwd: true
    };
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate().then(success => {
        if (success) {
          localStorage.setItem("token", "key");
          this.$router.push("/settings");
        } else {
          // oh no, user has filled in
          // at least one invalid value
        }
      });
    },
    onReset() {
      this.$refs.form.resetValidation();
    }
  }
};
</script>

<style>
</style>