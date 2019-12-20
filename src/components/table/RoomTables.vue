<template>
  <q-dialog v-model="getDialog" persistent full-width full-height>
    <q-card>
      <q-linear-progress :value="0" color="blue-10" />

      <q-card-section class="items-center no-wrap q-pa-md">
        <div class="row">
          <div class="col-md-3">
            <q-card flat class="my-card">
              <q-card-section>
                <div class="text-h5 text-weight-bold">{{ room.name }}</div>
              </q-card-section>
              <q-card-section>
                <div class="text-h6 text-grey-10">{{ room.type }}</div>
              </q-card-section>
              <q-card-section>
                <div class="text-caption">{{ room.description }}</div>
              </q-card-section>
              <q-separator />
              <q-card-section>
                <div class="text-h6">Table</div>
              </q-card-section>
              <q-card-section>
                <div class="row">
                  <div class="col-md-12 q-pa-xs">
                    <q-input filled v-model="selectedTable.name" type="text" label="Désignation" />
                  </div>
                  <div class="col-md-12 q-pa-xs">
                    <q-input
                      filled
                      v-model="selectedTable.chair"
                      type="text"
                      label="Nombre de couverts"
                    />
                  </div>
                  <div class="col-md-12 q-pa-xs">
                    <q-input
                      filled
                      v-model="selectedTable.description"
                      type="textarea"
                      label="Description"
                    />
                  </div>
                </div>
              </q-card-section>
            </q-card>
          </div>
          <div class="col-md-7">
            <q-card flat bordered class="my-card bg-grey-2">
              <q-card-section class="text-center scroll" style="max-height: 75vh">
                <svg
                  :width="room.width"
                  :height="room.height"
                  id="roomSVG"
                  @mouseout="stopMoving()"
                >
                  <rect width="100%" height="500" style="fill:rgb(240,240,240);" />
                  <g
                    v-for="table in tables"
                    :key="table.id"
                    style="cursor:pointer"
                    @click="selectTable(table.id)"
                  >
                    <rect
                      :width="table.width"
                      :height="parseFloat(table.height) + 5"
                      :rx="table.radius+'%'"
                      :ry="table.radius+'%'"
                      :transform="getRotationTransform(table)"
                      :x="table.x"
                      :y="table.y"
                      style="fill:rgb(200,200,200);"
                    />

                    <rect
                      :width="table.width"
                      :height="table.height"
                      :rx="table.radius+'%'"
                      :ry="table.radius+'%'"
                      :transform="getRotationTransform(table)"
                      :x="table.x"
                      :y="table.y"
                      style="fill:rgb(245,245,245);stroke-width:1;stroke:rgb(200,200,200)"
                    />
                    <text
                      :x="parseFloat(table.x) + parseFloat(table.width) / 2 - table.name.length*3"
                      :y="parseFloat(table.y) + parseFloat(table.height) / 2 + 5"
                      fill="#555"
                      font-size="12"
                    >{{ table.name }}</text>
                  </g>
                  <rect
                    width="100%"
                    height="500"
                    rx="5"
                    ry="5"
                    v-bind:style="{'fill': '#0d469f','opacity': '.7', 'display': (selectedTable.id!==null?'block':'none')}"
                    @click="deSelectTable()"
                  />
                  <g
                    v-if="selectedTable.id!==null"
                    style="cursor:pointer"
                    @click="deSelectTable()"
                    @mousedown="startMoving()"
                    @mousemove="moveTable(selectedTable)"
                    @mouseup="stopMoving()"
                  >
                    <rect
                      :width="selectedTable.width"
                      :height="parseFloat(selectedTable.height) + 5"
                      :rx="selectedTable.radius+'%'"
                      :ry="selectedTable.radius+'%'"
                      :transform="getRotationTransform(selectedTable)"
                      :x="selectedTable.x"
                      :y="selectedTable.y"
                      style="fill:rgb(200,200,200);"
                    />
                    <rect
                      :width="selectedTable.width"
                      :height="selectedTable.height"
                      :rx="selectedTable.radius+'%'"
                      :ry="selectedTable.radius+'%'"
                      :transform="getRotationTransform(selectedTable)"
                      :x="selectedTable.x"
                      :y="selectedTable.y"
                      style="fill:rgb(245,245,245);stroke-width:1;stroke:rgb(200,200,200)"
                    />
                    <text
                      :x="parseFloat(selectedTable.x) + parseFloat(selectedTable.width) / 2 - selectedTable.name.length*3"
                      :y="parseFloat(selectedTable.y) + parseFloat(selectedTable.height)  + 20"
                      fill="#FFF"
                      font-size="12"
                    >{{ selectedTable.name }}</text>
                  </g>
                </svg>
              </q-card-section>
            </q-card>
          </div>
          <div class="col-md-2">
            <q-card flat class="my-card">
              <q-card-section>
                <div class="text-h6">Outils</div>
              </q-card-section>
              <q-card-section>
                <div class="row">
                  <div class="col-md-12">
                    <div class="text-caption q-pa-xs">Taille</div>
                  </div>
                  <div class="col-md-6 q-pa-xs">
                    <q-input filled v-model="selectedTable.width" type="number" label="Longueur" />
                  </div>
                  <div class="col-md-6 q-pa-xs">
                    <q-input filled v-model="selectedTable.height" type="number" label="Largeur" />
                  </div>
                  <div class="col-md-12 q-pa-xs">
                    <div class="text-caption">Position</div>
                  </div>
                  <div class="col-md-6 q-pa-xs">
                    <q-input filled v-model="selectedTable.x" type="number" label="X" />
                  </div>
                  <div class="col-md-6 q-pa-xs">
                    <q-input filled v-model="selectedTable.y" type="number" label="Y" />
                  </div>
                  <div class="col-md-12 q-pa-xs q-mt-sm">Rotation</div>
                  <div class="col-md-12 q-pa-xs">
                    <q-slider v-model="selectedTable.rotation" :min="0" :max="360" color="blue-10" />
                  </div>
                  <div class="col-md-12 q-pa-xs q-mt-sm">Angles</div>

                  <div class="col-md-12 q-pa-xs">
                    <q-slider v-model="selectedTable.radius" :min="0" :max="50" color="blue-10" />
                  </div>
                </div>
              </q-card-section>
            </q-card>
          </div>
        </div>
      </q-card-section>
      <q-card-section class="no-wrap">
        <div class="row justify-end q-pa-md">
          <div class="q-gutter-md">
            <q-btn
              flat
              outlined
              class="q-pa-xs"
              color="grey-4"
              text-color="#111"
              type="submit"
              label="Annuler"
              v-close-popup
            />
            <q-btn
              push
              class="q-pa-xs"
              color="blue-10"
              text-color="white"
              type="submit"
              label="Enregistrer"
            />
          </div>
        </div>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>
<script>
export default {
  props: ["dialog", "roomID", "tables"],
  data() {
    return {
      room: {
        id: 1,
        name: "salle test",
        description: "Salle description",
        type: "VIP",
        width: 2000,
        height: 2000
      },
      /*tables: [
        {
          id: 1,
          label: "Table 1",
          room_id: 1,
          name: "Table 1",
          description: "sdfsdfsdf",
          x: 10,
          y: 10,
          width: 100,
          height: 100,
          rotation: 0,
          radius: 1,
          chair: 4
        },
        {
          id: 2,
          label: "Table 2",
          room_id: 1,
          name: "Table 2",
          description: "sdfsdfsdf",
          x: 10,
          y: 10,
          width: 100,
          height: 100,
          rotation: 0,
          radius: 1,
          chair: 4
        },
        {
          id: 3,
          label: "Table 3",
          room_id: 1,
          name: "Table 3",
          description: "dfsdfsdfsdf",
          x: 10,
          y: 10,
          width: 100,
          height: 100,
          rotation: 0,
          radius: 1,
          chair: 4
        }
      ],*/
      selectedTable: {
        id: null,
        label: "1",
        room_id: null,
        name: "",
        description: "",
        x: 0,
        y: 0,
        width: 0,
        height: 0,
        rotation: 0,
        radius: 0,
        chair: 0
      },
      moveAction: false
    };
  },
  created() {
    console.log(this.tables);
  },
  computed: {
    getDialog: {
      get: function() {
        return this.dialog;
      },
      set: function() {
        this.$emit("toggle");
      }
    }
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate().then(success => {
        if (success) {
        } else {
          // oh no, user has filled in
          // at least one invalid value
        }
      });
    },
    selectTable(tableID) {
      let selectedTable = this.tables.find(t => t.id === tableID);
      if (selectedTable) {
        this.selectedTable = selectedTable;
      }
    },
    deSelectTable() {
      this.selectedTable = {
        id: null,
        room_id: null,
        name: "",
        description: "",
        x: 0,
        y: 0,
        width: 0,
        height: 0,
        rotation: 0,
        radius: 0
      };
    },
    getRotationTransform(table) {
      return (
        "rotate(" +
        table.rotation +
        ", " +
        (parseFloat(table.x) + parseFloat(table.width) / 2) +
        ", " +
        (parseFloat(table.y) + parseFloat(table.height) / 2) +
        ")"
      );
    },
    startMoving() {
      this.moveAction = true;
    },
    stopMoving() {
      this.moveAction = false;
    },
    moveTable(table) {
      if (this.moveAction) {
        let roomSVG = document.querySelector("#roomSVG");
        let svgPos = roomSVG.getBoundingClientRect();
        this.selectedTable.x =
          window.event.clientX -
          svgPos.left -
          parseFloat(this.selectedTable.width) / 2;
        this.selectedTable.y =
          window.event.clientY -
          svgPos.top -
          parseFloat(this.selectedTable.height) / 2;
      }
    }
  }
};
</script>