<template>
  <div class>
    <div class="q-gutter-y-md q-pa-lg" style>
      <div class="text-h6">Salles</div>
      <div class="text-right">
        <q-btn
          push
          color="blue-10"
          class="q-pa-xs"
          text-color="white"
          label="Nouvelle salle"
          size="12px"
          @click="toggledialogROOM()"
        ></q-btn>
        <CreateROOM
          v-on:success="editRoomTables"
          v-on:toggle="toggledialogROOM"
          :dialog="dialogROOM"
        ></CreateROOM>
        <RoomTables
          v-on:toggle="toggledialogRoomTables"
          :dialog="dialogRoomTables"
          :roomID="roomID"
          :tables="getTables"
        ></RoomTables>
      </div>
    </div>
  </div>
</template>
<script>
import CreateROOM from "@/components/room/Create.vue";
import RoomTables from "@/components/table/RoomTables.vue";
export default {
  name: "room",
  data() {
    return {
      dialogROOM: false,
      dialogRoomTables: false,
      roomID: 1,
      tables: []
    };
  },
  computed: {
    getTables() {
      return this.tables;
    }
  },
  components: {
    CreateROOM,
    RoomTables
  },
  methods: {
    toggledialogROOM() {
      this.dialogROOM = !this.dialogROOM;
    },
    toggledialogRoomTables() {
      this.dialogRoomTables = !this.dialogRoomTables;
    },
    editRoomTables(data) {
      this.tables = data.tables;
      this.dialogRoomTables = true;
    }
  }
};
</script>