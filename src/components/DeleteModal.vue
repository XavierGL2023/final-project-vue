<template>
    <div class="modal">
      <div class="modal-content">
        <h2>Eliminar tarea</h2>
        <p>¿Estás seguro de que quieres eliminar la tarea "{{ tarea }}"?</p>
        <div class="modal-actions">
          <button @click="eliminarTarea">Eliminar</button>
          <button @click="cancelarEliminacion">Cancelar</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['tarea'],
    methods: {
      eliminarTarea() {
        // Lógica para eliminar la tarea
        this.$emit('eliminar-tarea', this.tarea);
      },
      cancelarEliminacion() {
        this.$emit('cancelar-eliminacion');
      },
    },
  };
  </script>
  
  <style>
  /* Estilos CSS para el modal */
  .modal {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 999;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .modal-content {
    background-color: white;
    padding: 1rem;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  }
  
  .modal-actions {
    display: flex;
    justify-content: flex-end;
    margin-top: 1rem;
  }
  </style>