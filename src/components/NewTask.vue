<template>
    <div class="add-task">
        <h3>Create New Task</h3>
        <button class="add_button" @click="addTask"></button>
    </div>
    <h1>Add a new Task</h1>
    <div v-if="showErrorMessage">
        <p class="error-text">{{ errorMessage }}</p>
    </div>
    <div class="task-field">
        <div class="input-field">
            <input type="text" placeholder="Task title" v-model="name">
        </div>
        <div class="input-field">
            <textarea type="text" placeholder="Task description" v-model="description"  maxlength="500" rows="3"></textarea>
        </div>
        <button class="add_button" @click="addTask"></button>
    </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task"   

const taskStore = useTaskStore();

// variables para los valors de los inputs
const name = ref('');
const description = ref('');

// constant to save a variable that holds an initial false boolean value for the errorMessage container that is conditionally displayed depending if the input field is empty
const showErrorMessage = ref(false);

// const constant to save a variable that holds the value of the error message
const errorMessage = ref(null);

// Arrow function para crear tareas.
const addTask = () => {
if(name.value.length === 0 || description.value.length === 0){
    // Primero comprobamos que ningún campo del input esté vacío y lanzamos el error con un timeout para informar al user.

    showErrorMessage.value = true;
    errorMessage.value = 'The task title or description is empty';
    setTimeout(() => {
    showErrorMessage.value = false;
    }, 5000);

} else {
    // Aquí mandamos los valores a la store para crear la nueva Task. Esta parte de la función tenéis que refactorizarla para que funcione con emit y el addTask del store se llame desde Home.vue.

    taskStore.addTask(name.value, description.value);
    name.value = '';
    description.value = '';
}
};

</script>

<style>
    .task-field {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    input[type="text"] {
    border-radius: 0.5rem;
    border-width: 1px;
    padding: 0.875rem 1.25rem;
    width: 300px;
    border: none;
    }

    textarea[type="text"] {
    border-radius: 0.5rem;
    border-width: 1px;
    padding: 0.875rem 1.25rem;
    width: 300px;
    border: none;
    }

    .add-task {
    margin: auto;
    margin-top: 15%;
    width: 300px;    
    background-color: #ECECEC;
    display: flex;
	flex-direction: row;
	flex-wrap: nowrap;
	justify-content: space-evenly;
	align-items: center;
	align-content: stretch;
    padding: 2%;
    border-radius: 1em;
    }

    .round-button {
  background-color: black;
  border: none;
  color: white;
  padding: 16px 24px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 50%;
  height: 70px;
  width: 70
px;
  line-height: 48px;
}

.round-button span {
  content: "+";
  display: block;
  font-size: 24px;
  line-height: 24px;
  margin-top: -12px;
}

.add_button {
    background-image: url(../assets/add_icon.svg);
    background-position: 50% 50%;
    background-size: auto;
    background-repeat: no-repeat;
}
</style>
  