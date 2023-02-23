<template>
    <div class="container-task">
        <div class="task-item">
        <h3 :class="props.task.is_complete ? 'clase1' : 'clase2'">{{task.title}}</h3>
        <p :class="props.task.is_complete ? 'clase1' : 'clase2'">{{task.description}}
        <!-- {{ task.is_complete }} -->
        </p>
        </div>
        <div class="buttons-task">
        <button class="delete_button" @click="deleteTask"></button>
        <button class="edit_button" @click="editTask"></button>
        <button class="ok_button" @click="completeTask"></button>
        <div v-if="showEdit">
            <div class="input-field">
            <input type="text" placeholder="Task title" v-model="nameChange">
        </div>
        <div class="input-field">
            <textarea type="text" placeholder="Task description" v-model="descriptionChange"  maxlength="500" rows="3"></textarea>
        </div>
        <button @click="sendData">Send data</button>
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

const props = defineProps({
    task: Object,
});

// Función para borrar la tarea a través de la store. El problema que tendremos aquí (y en NewTask.vue) es que cuando modifiquemos la base de datos los cambios no se verán reflejados en el v-for de Home.vue porque no estamos modificando la variable tasks guardada en Home. Usad el emit para cambiar esto y evitar ningún page refresh.
const deleteTask = async() => {
    // if canEdit -> then.delete. 
    await taskStore.deleteTask(props.task.id);
};

const completedTask = ref(false)
const toggleButton = () => {
    completedTask.value = !completedTask.value;
};

// EMIT BLOCK

const emit = defineEmits(['taskCompleteEmit'])

// funcion para completar tarea

const completeTask = () => {
    console.log("I have been clicked")
    console.log(props.task)
    console.log(props.task.is_complete)
    emit('taskCompleteEmit', props.task)
}

const editTask = () => {
    showEdit.value = true;
}

const sendData = () => {
    taskStore.updateTask(props.task.id, nameChange.value, descriptionChange.value);
    showEdit.value = false;
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
