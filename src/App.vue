<template>
  <div id="app">
    <div class="row">
      <div class="col s12">
        <p class="flow-text center-align green-text">Mi gestor de tareas</p>
      </div>
    </div>
    <div class="row">
      <div class="input-field col s12">
        <input v-model="tituloLista" placeholder="Escribe una lista" @keyup.enter="anadirLista" />
      </div>

      <div class="col s12">
        <button class="btn green" @click="anadirLista" style="width: 100%">Añadir Lista</button>
      </div>
    </div>
  </div>
  <div class="row" v-for="(lista, index) in listas" :key="lista.id">
    <div class="col s12 m6 l4">
      <div class="card yellow lighten-4 z-depth-2">
        <div class="card-content">
          <span class="card-title"></span>
          <List
            :tituloLista="lista.title"
            :id="lista.id"
            @eliminarLista="eliminarLista"
            @actualizarListas="guardarListas"
          />
        </div>
        <div class="card-action">
          <button class="btn red" @click="eliminarLista(lista.id)">Eliminar lista</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import List from './components/List.vue'

export default {
  name: 'App',
  components: {
    List,
  },
  data() {
    return {
      tituloLista: '',
      listas: [],
    }
  },

  mounted() {
    const listasGuardadas = localStorage.getItem('listas')
    if (listasGuardadas) {
      this.listas = JSON.parse(listasGuardadas)
    }
  },

  methods: {
    anadirLista() {
      if (this.tituloLista.trim() !== '') {
        this.listas.push({
          title: this.tituloLista,
          tareas: [],
          id: Date.now(),
        })
        this.tituloLista = ''
        this.guardarListas()
        const modal = M.Modal.getInstance(document.getElementById('modalLista'))
        modal.open()
      }
    },

    eliminarLista(id) {
      localStorage.removeItem(`tareas-${id}`)

      this.listas = this.listas.filter((lista) => lista.id !== id)

      this.guardarListas()
    },

    guardarListas() {
      localStorage.setItem('listas', JSON.stringify(this.listas))
    },
  },
}
</script>
