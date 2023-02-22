<template>
  <div class="wrapper">
    <Nav />
    <div class="task-top">
    <!-- Button to show/hide newTask comp -->
    <div class="add-task">
      <h3>Create New Task</h3>
      <button class="add_button" @click="showNewTaskComp"></button>
    </div>   
    <NewTask v-if="showHideTaskVar" />
    <h1 class="tasksh1">Tasks:</h1>
  </div>
    <div class="mi-cool-div">
      <TaskItem 
      v-for="task in tasks" 
      :key="task.id" :task="task" 
      @task-complete-emit="taskCompleteSupa"
      />
    </div>
    <Footer />
  </div>
</template>

<script setup>
import { ref, onUpdated } from "vue";
import { useTaskStore } from "../stores/task";
import { useRouter } from "vue-router";
import Nav from "../components/Nav.vue";
import NewTask from "../components/NewTask.vue";
import TaskItem from "../components/TaskItem.vue";
import Footer from "../components/Footer.vue";

const taskStore = useTaskStore();

// Variable para guardar las tareas de supabase
const tasks = ref([]);

// Creamos una función que conecte a la store para conseguir las tareas de supabase
const getTasks = async () => {
  tasks.value = await taskStore.fetchTasks();
};

getTasks();

onUpdated(() => {
  getTasks();
});

// show hide logic

// varbooleana para controlar el estado visbile del comp
let showHideTaskVar = ref(true);

const showNewTaskComp = () => {
  showHideTaskVar.value = !showHideTaskVar.value;
  console.log(showHideTaskVar);
};


// Function to connect to supabase and change the value of the task from is_complete=false to is_complete=true
const taskCompleteSupa = async (taskInformation) => {
  // vamos a crear una var que apunte al param taskInformation y usando un poco de concatenacion vamos a sementar el valor de is_cpmplete. UNa vez recibamos el valor como ya VIMOS XAVI vamos a segementar el valor opuesto
  const toggleTaskStatus = !taskInformation.is_complete
  // vamos a crear una var que apunte al param taskInformation y usando un poco de concatenacion vamos a sementar el valor del id de la tarea
  const taskId = taskInformation.id
  console.log(taskId, toggleTaskStatus)

  // vamos a conectarnos con supabase :) 
  await taskStore.markTaskCompleted(taskId, toggleTaskStatus)
}

</script>

<style>
.mi-cool-div {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
  margin-left: 15%;
  margin-right: 15%;
  background-color: #FFDD0B;
}
.tasksh1{
  margin-left: 8%;
  margin-bottom: 0px;
}

.task-top {
  margin-left: 15%;
    margin-right: 15%;
    background-color: #FFDD0B;
}

</style>

<!-- 
**Hints**
1. ref() is used here!
2. (NewTask, TaskItem, Footer, Nav) components are used here! 
3. Tasks are going to be contained in an array here!
4. An async function is needed to get all of the tasks stored within the supabase database, this async function's body will 
contain the tasks value which be use to store the fetchTasks method which lives inside the userTaskStore. This function 
needs to be called within the setUp script in order to run within the first instance of this component lifecycle.

5. NewTask component will receive a customEvent on this instance of the homeView that will fire the add-to-do function
6. add-to-do function will receive 2 params/arguments that will tak a taskTitle and a taskDescription and the body of this 
async function will call the taskStore that calls the addTask function from the store that pushes the info of the task to the 
backEnd. This is possible by passing the 2 param/arguments that will be passed by the user from the inputs within the NewTask 
Component. 

7. TaskItem component will loop through the tasks-array that will print an individual instance of an individual TaskItem 
component. TaskItem will receive 3 customEvents on this instance of the homeView. 1 customEvent for toggling the task to show 
either a text or an icon to display if the task is completed or not completed. 1 customEevent for removing/deleting the 
task out of the array. 1 customEvent for editing the task title and description. This function needs to call the function 
mentioned on hint4.


7.1-customEvent will fire an async function that will take in 1 param/argument. On the body of this function the param/argument 
will be used to define 2 constants. 1 of this constants will take care of setting the boolean value to the opposite of the 
value that checks wether this task is_complete. 1 of this constants will take of calling the id of this specific task in 
order to call the right id. 
7.2-customEvent will fire an asynf function that will take in 1 param/argument. This async function's body will be used to 
call the deleteTaskmethod which will take the param/argument's id in order to delete the task. This function needs to call 
the function mentioned on hint4. 
7.3-customEvent will fire an async function that will take in 1 param/argument. this async function's body will be used to 
take in 2 constants. 1 constant will take in the param/argument newValue. 1 constant will be used to get the param/argument 
oldValue id. These 2 constants will be sent to the backend via the useTaskStore which holds an editTask method. This function 
needs to call the function mentioned on hint4.
-->
