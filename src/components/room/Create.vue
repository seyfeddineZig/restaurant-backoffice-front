<template>
  <q-dialog v-model="getDialog" persistent>
    <q-card style="width: 500px; max-width: 50vw;">
      <q-linear-progress :value="0" color="blue-10" />
      <q-card-section class="row items-center">
        <div class="text-h6">Nouvelle salle</div>
        <q-space />
        <q-btn icon="far fa-times-circle" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section class="row items-center no-wrap q-pa-md">
        <q-form ref="form" @submit="onSubmit" class="q-gutter-md">
          <div class="row">
            <div class="col-md-12 q-pa-xs q-mt-sm">
              <q-input
                v-model="name"
                class="q-p-l-4"
                filled
                color="blue-10"
                type="text"
                label="Désignation"
                lazy-rules
                :rules="[
                  val =>
                    (val && val.length > 0) ||
                    'Veuillez saisir le nom de la famille'
                ]"
              ></q-input>
            </div>
            <div class="col-md-6 q-pa-xs q-mt-sm">
              <q-input
                v-model="tableCount"
                class="q-p-l-4"
                filled
                color="blue-10"
                type="number"
                label="Nombre de tables"
                lazy-rules
                :rules="[
                  val =>
                    (val &&
                      val.length > 0 &&
                      !isNaN(val) &&
                      parseInt(val) >= 0) ||
                    'Veuillez saisir un nombre valide'
                ]"
              ></q-input>
            </div>
            <div class="col-md-6 q-pa-xs q-mt-sm">
              <q-input
                v-model="defaultPrice"
                class="q-p-l-4"
                filled
                color="blue-10"
                type="text"
                label="Tarif par defaut"
                lazy-rules
                :rules="[
                  val =>
                    (val &&
                      val.length > 0 &&
                      !isNaN(val) &&
                      parseFloat(val) >= 0) ||
                    'Veuillez saisir un nombre valide'
                ]"
              ></q-input>
            </div>
            <div class="col-md-12 q-pa-xs q-mt-sm">
              <q-input
                v-model="description"
                class="q-p-l-4"
                filled
                color="blue-10"
                type="textarea"
                label="Description"
                lazy-rules
                :rules="[]"
              ></q-input>
            </div>
            <div class="col-md-6 q-pa-xs q-mt-sm">
              <q-input v-model="width" filled color="blue-10" type="number" label="Longueur" />
            </div>
            <div class="col-md-6 q-pa-xs q-mt-sm">
              <q-input v-model="height" filled color="blue-10" type="number" label="Largeur" />
            </div>
            <div class="col-md-8 q-pa-xs q-mt-sm">
              <q-toggle v-model="isActive" color="blue-10" label="Afficher la salle sur le site" />
            </div>
            <div class="col-md-4 q-pa-xs q-mt-sm">
              <q-toggle v-model="isVIP" color="blue-10" label="Salle VIP" />
            </div>
          </div>

          <div class="row justify-end q-mt-lg">
            <div class="q-gutter-sm">
              <q-btn
                push
                class="q-pa-xs"
                color="blue-10"
                text-color="white"
                type="submit"
                label="Valider"
              />
            </div>
          </div>
        </q-form>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>
<script>
export default {
  props: ["dialog"],
  data() {
    return {
      name: "",
      tableCount: 0,
      description: "",
      bgColor: "#0d47a1",
      textColor: "#FFFFFF",
      isVIP: false,
      isActive: true,
      defaultPrice: "0",
      width: 500,
      height: 500,
      tables: []
    };
  },
  computed: {
    getDialog: {
      get: function() {
        return this.dialog;
      },
      set: function() {
        this.empty();
        this.$emit("toggle");
      }
    }
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate().then(success => {
        if (success) {
          for (let i = 0; i < this.tableCount; i++) {
            let row = this.tableCount <= 6 ? 6 : Math.ceil(this.tableCount / 4);
            let tableWidth = this.width / row - 10;
            let table = {
              id: i,
              name: "Table " + (i + 1),
              width: tableWidth,
              height: tableWidth,
              x: (i % row) * (tableWidth + 10),
              y: Math.floor(i / row) * (tableWidth + 10),
              rotation: 0,
              radius: 1,
              chair: 4
            };
            /*table.id = i;
            table.name = "Table " + (i + 1);
            table.x = (5 + table.width) * (i % Math.ceil(this.tableCount / 4));
            table.y = (5 + table.height) * Math.ceil(i / (this.tableCount / 4));*/
            this.tables.push(table);
          }
          this.$emit("success", { tables: this.tables });
        } else {
          // oh no, user has filled in
          // at least one invalid value
        }
      });
    },
    empty() {
      this.name = "";
      this.tableCount = 0;
      this.description = "";
      this.bgColor = "#0d47a1";
      this.textColor = "#FFFFFF";
      this.isVIP = false;
      this.isActive = true;
      this.defaultPrice = "0";
    }
  }
};
</script>
