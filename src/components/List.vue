<template>
  <span class="titulo-lista">{{ tituloLista }}</span>
  <div class="row">
    <div class="input-field col s12">
      <input
        type="text"
        v-model="tituloTarea"
        placeholder="Escribe una nueva tarea"
        @keyup.enter="anadirTarea"
      />
    </div>
    <div class="col s12">
      <button class="btn green" @click="anadirTarea">Agregar Tarea</button>
    </div>
  </div>

  <div class="ListaTareas">
    <div class="row">
      <div class="col s12" v-for="(tarea, id) in tareas" :key="tarea.id">
        <div class="card blue-grey lighten-5 z-depth-1">
          <div class="card-action">
            <ListItem :tituloTarea="tarea.titulo" :id="tarea.id" @eliminarTarea="eliminarTarea" />
          </div>
        </div>
        <div class="left-align" style="margin-top: 4px; margin-bottom: 24px">
          <button class="btn red" @click="eliminarTarea(tarea.id)">Eliminar tarea</button>
        </div>
      </div>
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
        this.tareas.push({
          titulo: this.tituloTarea,
          id: Date.now(),
        })
        this.tituloTarea = ''
        this.guardarTareas()
        this.modalVisible = true

        const modal = document.getElementById('modalTarea')
        const instance = M.Modal.init(modal)
        instance.open()
      }
    },
    eliminarTarea(id) {
      this.tareas = this.tareas.filter((tarea) => tarea.id !== id)
      this.guardarTareas()
    },
    guardarTareas() {
      localStorage.setItem(`tareas-${this.id}`, JSON.stringify(this.tareas))
    },
  },
  mounted() {
    const tareasGuardadas = localStorage.getItem(`tareas-${this.id}`)
    if (tareasGuardadas) {
      this.tareas = JSON.parse(tareasGuardadas)
    }
  },
}
</script>
