<template>
  <q-page padding>
    <div class="text-h2 text-grey-8 ">
      TO DO LIST
      <span>
        <q-img src="../assets/todolist.png" style="height:100px;width:100px" />
      </span>
    </div>

    <div class="row q-mt-sm">
      <div class="column col-12  items-center justify-evenly ">
        <q-card class="card-header ">
          <q-card-section>
            <div class="text-h6">Tareas</div>
          </q-card-section>
          <q-separator></q-separator>
          <q-card-section id="task-container" class="body-card">
            <q-banner
              v-for="(tarea, index) in tareas"
              :key="index"
              class="bg-primary text-white q-mb-sm"
            >
              <span class="text-h4"> {{ tarea }}</span>
              <template v-slot:action>
                <q-btn
                  flat
                  color="white"
                  label="Eliminar"
                  @click="eliminarTarea(index)"
              /></template>
            </q-banner>
          </q-card-section>
          <q-separator></q-separator>
          <q-card-actions class="  row justify-end">
            <q-btn
              size="15px"
              class="glossy"
              round
              color="deep-orange"
              icon="add"
              @click="prompt = true"
            />
          </q-card-actions>
        </q-card>
      </div>
    </div>

    <q-dialog v-model="prompt" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Nueva Tarea</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input
            dense
            autofocus
            @keyup.enter="prompt = true"
            id="tarea-descripcion"
            v-model="tareaDescripcion"
          />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancelar" v-close-popup />
          <q-btn
            id="btn-agregar"
            flat
            label="Agregar tarea"
            v-close-popup="status"
            @click="addTask"
          />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
/*eslint-disable */
export default {
   /*eslint-disable */
  data() {
    return {
      alert: false,
      confirm: false,
      prompt: false,
      tareaDescripcion: "",
      tareas: [""],
      status: true
    };
  },
  methods: {
    addTask() {
      /*eslint-disable */
      this.status = true;
      if (this.tareaDescripcion === "") {
        alert("Debe suministrar un nombre de tarea valido");
        this.status = false;
      } else {
        this.registrarTareas(this.tareaDescripcion);
        this.tareaDescripcion = "";
      }
    },

    registrarTareas(tarea) {
      /*eslint-disable */
      const index = this.tareas.length + 1;
      this.tareas.push(tarea);
      window.localStorage.setItem(index.toString(), tarea);
    },

    mostra_tareas() {
      this.tareas = Object.values(localStorage);
      console.log(this.tareas);
    },
    eliminarTarea(item) {
      let index = item + 1
      this.tareas.splice(item, 1);
      
      window.localStorage.removeItem(index.toString());
      
    }
  },
  created() {
    this.mostra_tareas();
  }
};
</script>
<style scoped>
.card-header {
  width: 70%;
  min-height: 400px;
}
.body-card {
  width: 100%;
  min-height: 400px;
}
</style>
