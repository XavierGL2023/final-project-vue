<template>
    <div class="container-task">
        <div class="task-item">
        <h3 :class="props.task.is_complete ? 'clase1' : 'clase2'">{{task.title}}</h3>
        <p :class="props.task.is_complete ? 'clase1' : 'clase2'">{{task.description}}
        <!-- {{ task.is_complete }} -->
        </p>
        </div>
        <div class="buttons-task">
            <button v-if="hide_boolean" class="edit_button" @click="editTask"></button>
            <button v-if="!hide_boolean" class="ok_button" @click="completeTask"></button>
            <button v-else class="ok_return_button" @click="completeTask"></button>
            <button v-if="hide_boolean" class="delete_button" @click="showModalToggle"></button>
        <div class="modalcontainer">
      <div class="modal-mask" v-if="showModal">
         <div class="modal-wrapper">
        <div class="modal-container">

          <div class="modal-header">
            <slot name="header">
              Are you sure you want to delete the task "{{ task.title }}" ?
            </slot>
          </div>  

          <div class="modal-body">
            <!--<slot name="body">
              default body
            </slot>-->
          </div>

          <div class="modal-footer">
            <slot name="footer">
              <button class="botonokmod" @click="deleteTask">Yes</button>
              <button class="botoncancmod" @click="showModalToggle">Cancel</button>
            </slot>
          </div>
          </div>
        </div>
      </div>
      <!-- <h2>Are you sure you want to delete the task "{{ task.title }}" ?</h2>
      <button @click="deleteTask">Yes</button>
      <button @click="showModalToggle">Cancel</button> -->
      </div>

        <div class="modal-mask" v-if="showModalEdit">
      <div class="modal-wrapper">
        <div class="modal-container">

          <div class="modal-header">
            <slot name="header">
              Here you can edit your task "{{ task.title }}" 
            </slot>
          </div>
            <div class="input-field">
                <input type="text" placeholder="Task title" v-model="nameChange">
            </div>
            <div class="input-field">
                <textarea type="text" placeholder="Task description" v-model="descriptionChange"  maxlength="500" rows="3"></textarea>
            </div>
            <div class="modal-footer">
            <slot name="footer">
              <div class="absolutePosition" v-if="showErrorMessage">
                <p class="error-text">{{ errorMessage }}</p>
              </div>

              <button class="buttonSendData" @click="sendData">Confirm</button>
            </slot>
          </div>
            <!--<button @click="sendData">Send data</button>-->
        </div>
        </div>
    </div>
    </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { useTaskStore } from '../stores/task';
import { supabase } from '../supabase';

const taskStore = useTaskStore();
const showEdit = ref(false);
const nameChange = ref("");
const descriptionChange = ref("");
const canEdit = ref(!props.task.is_complete);
const hide_boolean = ref(true)
const showModalEdit = ref(false);
const showModalInputToggle = () =>{
    showModalEdit.value = !showModalEdit.value;
};



const props = defineProps({
    task: Object,
});

// Función para borrar la tarea a través de la store. El problema que tendremos aquí (y en NewTask.vue) es que cuando modifiquemos la base de datos los cambios no se verán reflejados en el v-for de Home.vue porque no estamos modificando la variable tasks guardada en Home. Usad el emit para cambiar esto y evitar ningún page refresh.
const deleteTask = async() => {
    // if canEdit -> then.delete. 
    await taskStore.deleteTask(props.task.id);
    // Afegit 23/02
    showModalToggle();
};

const completedTask = ref(false)
const toggleButton = () => {
    completedTask.value = !completedTask.value;
};
// Afegit 23/02
const showModal = ref(false);
const showModalToggle = () => {
    showModal.value = !showModal.value;
}

// EMIT BLOCK

const emit = defineEmits(['taskCompleteEmit'])

// funcion para completar tarea

const completeTask = () => {
    console.log("I have been clicked")
    console.log(props.task)
    console.log(props.task.is_complete)
    emit('taskCompleteEmit', props.task)
    hide_boolean.value = !hide_boolean.value
}

const editTask = () => {
    showEdit.value = true;
    showModalInputToggle();
}

const sendData = () => {
    taskStore.updateTask(props.task.id, nameChange.value, descriptionChange.value);
    showEdit.value = false;
    showModalEdit.value = !showModalEdit.value;
}

</script>

<style>

.hover-bg {
    background-color: white;
  }

.container{
background-color: #F4AD28;
flex-basis: 25%;
padding: 1%;
margin: auto;
margin-bottom: 10%;
width: 30%; 
    display: flex;
	flex-direction: column;
	flex-wrap: nowrap;
	justify-content: space-evenly;
	align-items: center;
	align-content: stretch;
    border: 3px solid #000000;
    box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
}
.container-task{
background-color: #F4AD28;
margin: auto;
margin-bottom: 10%;
width: 95%; 
    display: flex;
	flex-direction: column;
	flex-wrap: nowrap;
	justify-content: space-evenly;
	align-items: center;
	align-content: stretch;
    border: 3px solid #000000;
    border-radius: 15px;
    box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
}

.buttons-task {
    margin: auto;
    margin-top: 10%;
    space-between: 10px;
}
.clase2{
    text-decoration: none;
    margin-top: 20px;
    text-align: left;
}
.clase1{
    text-decoration: line-through;
    margin-top: 20px;
    text-align: left;
}

button {
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
width: 70px;
line-height: 48px;
box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
}

img {
    width:25px;
}

.ok_button {
    background-image: url(../assets/return.svg);
    background-position: 50% 50%;
    background-size: auto;
    background-repeat: no-repeat;
    margin: 20px 10px;
    background-size: 45%;
}
.ok_return_button {
    background-image: url(../assets/ok_icon.svg);
    background-position: 50% 50%;
    background-size: auto;
    background-repeat: no-repeat;
    margin: 20px 10px;
}
.delete_button {
    background-image: url(../assets/delete_icon.svg);
    background-position: 50% 50%;
    background-size: auto;
    background-repeat: no-repeat;
    margin: 20px 10px;
}
.edit_button {
    background-image: url(../assets/edit_icon.svg);
    background-position: 50% 50%;
    background-size: auto;
    background-repeat: no-repeat;
    margin: 20px 10px;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 5px;
  border-color: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

</style>

<!--
**Hints**
1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or
like an object, up to you.

2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error,
a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit
the new task detail or details[this is in reference of the task title and the task description].

3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the
status[completed, not complted] of the taskItem.

4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean
empty variable.

5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the
inputField will be used here to save the value as a prop on this function.

6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.

7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional
that first checks if the value of the task [either title and description or just title] is empty which if true it runs the
function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2
back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2;
a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data
property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value
from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field
to an empty string to clear it from the ui.

8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the removal of  the task on the homeview.
-->
