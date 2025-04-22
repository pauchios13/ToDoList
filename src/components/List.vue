<template>
  <span class="titulo-lista">{{ tituloLista }}</span>

  <div class="row tareas">
    <div class="input-field col s12 m9">
      <input
        type="text"
        v-model="tituloTarea"
        placeholder="Escribe una nueva tarea"
        @keyup.enter="anadirTarea"
      />
    </div>
    <div class="col s12 m3">
      <button class="btn green waves-effect waves-light full-width" @click="anadirTarea">
        Agregar Tarea
      </button>
    </div>
  </div>

  <div class="ListaTareas">
    <div v-for="(tarea, index) in tareas" :key="tarea.id" class="section tarea-item">
      <ListItem :tituloTarea="tarea" :id="index" @eliminarTarea="eliminarTarea" />
    </div>
  </div>

  <div id="modalTarea" class="modal">
    <div class="modal-content">
      <h4>Tarea añadida</h4>
      <p>Se ha añadido una tarea nueva</p>
    </div>
    <div class="modal-footer">
      <a href="#!" class="modal-close waves-effect waves-green btn-flat">Aceptar</a>
    </div>
  </div>
</template>

<script>
import ListItem from './ListItem.vue'

export default {
  components: {
    ListItem,
  },
  props: {
    tituloLista: String,
    id: Number,
  },
  data() {
    return {
      tituloTarea: '',
      tareas: [],
    }
  },
  methods: {
    anadirTarea() {
      if (this.tituloTarea.trim() !== '') {
        this.tareas.push(this.tituloTarea)
        this.tituloTarea = ''
        this.modalTarea.open()
        this.guardarTareas()
      }
    },
    eliminarTarea(index) {
      this.tareas.splice(index, 1)
      this.guardarTareas()
    },
    guardarTareas() {
      localStorage.setItem(`tareas-${this.id}`, JSON.stringify(this.tareas))
    },
  },
  mounted() {
    const modalElem = document.getElementById('modalTarea')
    this.modalTarea = M.Modal.init(modalElem)
    const tareasGuardadas = localStorage.getItem(`tareas-${this.id}`)
    if (tareasGuardadas) {
      this.tareas = JSON.parse(tareasGuardadas)
    }
  },
}
</script>

<style scoped>
.titulo-lista {
  display: block;
  font-weight: bold;
  color: #1565c0;
  margin-bottom: 15px;
  margin-top: 40px;
}

.tareas {
  margin-bottom: 20px;
}

.full-width {
  width: 100%;
}

.tarea-item {
  background-color: #e2545400;
  border-radius: 8px;
  padding: 12px 16px;
  margin-bottom: 10px;
  border: 1px solid #e0e0e0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
