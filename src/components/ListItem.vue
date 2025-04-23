<template>
  {{ tituloTarea }}
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
