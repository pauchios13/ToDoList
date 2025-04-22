<template>
  <div id="app" class="container">
    <h1 class="center-align">Mi To-Do</h1>

    <div class="row">
      <div class="input-field col s12 m8">
        <input v-model="tituloLista" @keyup.enter="anadirLista" />
      </div>

      <div class="col s12 m6 l3">
        <button class="btn green waves-effect waves-light full-width" @click="anadirLista">
          Añadir Lista
        </button>
      </div>
    </div>

    <div class="row" v-for="(lista, index) in listas" :key="index">
      <div class="col s12 m9">
        <List :tituloLista="lista.title" :id="index" @eliminarLista="eliminarLista" />
      </div>
      <div class="col s12 m3">
        <button class="btn red waves-effect waves-light full-width" @click="eliminarLista(index)">
          Eliminar lista
        </button>
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
        })
        this.tituloLista = ''
        this.guardarListas()
        const modal = M.Modal.getInstance(document.getElementById('modalLista'))
        modal.open()
      }
    },

    eliminarLista(index) {
      this.listas.splice(index, 1)
      this.guardarListas()
    },

    guardarListas() {
      localStorage.setItem('listas', JSON.stringify(this.listas))
    },
  },
}
</script>
<style scoped>
.row {
  margin-bottom: 20px;
}
</style>
