<template>
  <div>
    <button @click="showComponent = !showComponent">Mostrar/Ocultar Componente</button>
    <div v-show="showComponent">
      <MiComponente />
    </div>
  </div>
</template>

<script>
import NewTask from '@/components/NewTask.vue'

export default {
  name: 'MiVista',
  components: {
    NewTask
  },
  data() {
    return {
      showComponent: false
    }
  }
}
</script>