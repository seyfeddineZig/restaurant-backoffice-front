<template>
  <div class>
    <q-layout view="lHh Lpr lff" container class="window-height shadow-2 rounded-borders">
      <q-header class="bg-blue-10">
        <q-toolbar>
          <q-toolbar-title>Oxymore</q-toolbar-title>
          <q-btn
            @click="drawer = !drawer"
            rounded
            push
            color="white"
            text-color="blue-10"
            label
            size="12px"
          />
        </q-toolbar>
      </q-header>

      <q-drawer
        v-model="drawer"
        show-if-above
        :width="300"
        :breakpoint="430"
        content-class="bg-dark"
      >
        <q-scroll-area style="height: calc(100% - 150px); margin-top: 100px;">
          <!--<q-list padding>
            <q-item clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="inbox" />
              </q-item-section>

              <q-item-section>Inbox</q-item-section>
            </q-item>

            <q-item active clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="star" />
              </q-item-section>

              <q-item-section>Star</q-item-section>
            </q-item>

            <q-item clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="send" />
              </q-item-section>

              <q-item-section>Send</q-item-section>
            </q-item>

            <q-item clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="drafts" />
              </q-item-section>

              <q-item-section>Drafts</q-item-section>
            </q-item>
          </q-list>-->
          <div class="q-pa-lg">
            <div class="row">
              <div class="col-md-12 q-mb-lg">
                <div class="text-body1 text-grey-6">Menu</div>
              </div>
              <div class="col-md-6 text-center">
                <q-btn
                  text-color="blue-10"
                  push
                  outline
                  class="q-pa-sm q-mb-sm q-pt-md q-pb-md"
                  rounded
                >
                  <q-icon :name="'fas fa-tachometer-alt'" size="26px" />
                </q-btn>
                <div class="text-caption text-white">Tableau de bord</div>
              </div>
              <div class="col-md-6 text-center">
                <q-btn
                  text-color="white"
                  push
                  color="blue-10"
                  class="q-pa-sm q-mb-sm q-pt-md q-pb-md"
                  rounded
                >
                  <q-icon :name="'fas fa-utensils'" size="26px" />
                </q-btn>
                <div class="text-caption text-white">Réservations</div>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 q-mt-lg q-mb-lg">
                <div class="text-body1 text-grey-6">Paramètres</div>
              </div>
              <div class="col-md-6 text-center">
                <q-btn
                  text-color="white"
                  push
                  outline
                  class="q-pa-sm q-mb-sm q-pt-md q-pb-md"
                  rounded
                  :to="{ name: 'room'}"
                >
                  <q-icon :name="'fas fa-th'" size="26px" />
                </q-btn>
                <div class="text-caption text-white">Salles et tables</div>
              </div>
              <div class="col-md-6 text-center">
                <q-btn
                  text-color="white"
                  push
                  outline
                  class="q-pa-sm q-mb-sm q-pt-md q-pb-md"
                  rounded
                >
                  <q-icon :name="'fas fa-users'" size="26px" />
                </q-btn>
                <div class="text-caption text-white">Clients</div>
              </div>
            </div>
            <div class="row">
              <div class="col-md-6 text-center q-mt-md">
                <q-btn
                  push
                  color="white"
                  class="q-pa-sm q-mb-sm q-pt-md q-pb-md"
                  rounded
                  outline
                  text-color="white"
                >
                  <q-icon :name="'fas fa-users-cog'" size="26px" />
                </q-btn>
                <div class="text-caption text-white">Utilisateurs</div>
              </div>
              <div class="col-md-6 text-center q-mt-md">
                <q-btn
                  push
                  color="white"
                  class="q-pa-sm q-mb-sm q-pt-md q-pb-md"
                  rounded
                  outline
                  text-color="white"
                >
                  <q-icon :name="'fas fa-cogs'" size="26px" />
                </q-btn>
                <div class="text-caption text-white">Paramètres</div>
              </div>
            </div>
          </div>
        </q-scroll-area>

        <q-img class="absolute-top" style="height: 90px">
          <div class="absolute-bottom bg-transparent">
            <!--<q-avatar size="56px" class="q-mb-sm">
              <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
            </q-avatar>-->
            <div class="text-weight-bold">Razvan Stoenescu</div>
            <div>@rstoenescu</div>
          </div>
        </q-img>
      </q-drawer>

      <q-page-container>
        <q-page>
          <router-view></router-view>
        </q-page>
      </q-page-container>
    </q-layout>
  </div>
</template>
<script>
export default {
  data() {
    return {
      drawer: true,
      nightMode: false
    };
  },
  computed: {
    getNightMode(color) {
      if (this.nightMode) {
        return true;
      } else {
        return color;
      }
    }
  },
  created() {},
  methods: {}
};
</script>
<style>
</style>