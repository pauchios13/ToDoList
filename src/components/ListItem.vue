<template>
  {{ tituloTarea }}

  <button class="btn red waves-effect waves-light eliminar-btn" @click="eliminarTarea">
    Eliminar Tarea
  </button>
</template>
<script>
export default {
  props: {
    tituloTarea: String,
    id: Number,
  },
  methods: {
    eliminarTarea() {
      this.$emit('eliminarTarea', this.id)
    },
  },
}
</script>
