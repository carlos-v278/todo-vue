
<template>
  <div id="todo" :class="{dark_mode:darkMode}">
    <div class="header">
    
    </div>
      <div class="todo_app">
          <div class="header_todo">
            <h1 class="title">todo <span @click="switchDarkMode()" class="switch_dw"></span></h1>
            <div class="new_task task">
              <button class="btn_save btn" @click="addTask(currentTask)" type=""><img  src="" alt=""></button><input @keydown.enter="addTask(currentTask)" class="init_task input" v-model="currentTask" placeholder="new task" type="text">
            </div>
          </div>
          <div class="todo_container">
            <ul class="todo_lists">
            <div class="cont_todo_item" v-for="(task,index) in tasks" :key="index" >
              <li class="todo_item task" v-if="task.done == filter || filter == 'all'"  >
                 <button class="btn_check btn" @click="taskDone(index)" :class="{btn_done:task.done}" type=""  ></button> 
                 <input class="edit_task input" :class="{line_throu:task.done}" v-model="task.content"  type="text"><img @click="deleteTask(index)" class="cross_input" src="./assets/images/icon-cross.svg" alt="">
               </li>
            

            </div>
          
            </ul>
          <div class="manage" v-if="{tasks:false}">
            <span class="tasks_to_make">Tasks to do {{tasksDone.length}}</span>
            <ul class="filter">
              <li @click="filterTodo('all')" >All</li>
              <li @click="filterTodo('active')" >Active</li>
              <li @click="filterTodo('completed')" >Completed</li>
            </ul>
            <span class="clear_tasks" @click="deleteTasks()">Clear tasks</span>
          </div>
        </div>
      </div>
    <div class="footer">
    
    </div>

  </div>

</template>

<script setup>
import { ref, reactive, watch,onMounted  } from 'vue'

const currentTask = ref('')
let darkMode = ref(false)
let filter = ref('all')
const tasks = reactive([])

let tasksDone = ref({})

watch(tasks, (newValue) => {
  localStorage.tasks = JSON.stringify(newValue)
})



onMounted(() => {
    if(!localStorage.tasks == []) {
      let object = JSON.parse(localStorage.tasks)
      object.forEach(element => {
        tasks.push(element)
      });
    } 
})


const addTask = (task)=>{
 if(!task == ""){
  tasks.push({
    content:task,
    done:false
  })
  currentTask.value = ''
 }
}
const switchDarkMode = ()=>{
  
  if(darkMode.value){
    darkMode.value = false
  
  } else {
    darkMode.value = true
  }

}
const filterTodo = (fil)=>{
  if(fil == 'active'){
    filter.value = false
  } else if(fil == 'completed'){
    filter.value = true
  } else {
    filter.value = 'all'
  }

}

const taskDone = (index)=>{
  tasks.forEach((element,i) => {
    if(index === i) {
      if(element.done == false){
        element.done = true
        
      } else {
        element.done =false
      }
    }
    
  });
 
  tasksDone.value = tasks.filter(task => task.done == false)

}
const deleteTask = (index)=>{
  tasks.forEach((element,i) => {
    if(index === i) {
      tasks.splice(i ,1)
    }
    
  });
}


const deleteTasks = ()=>{
 tasks.splice(0,tasks.length)
}


</script>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;300;400;500;600;700&display=swap');
  #todo{
    font-family: 'Poppins', sans-serif;
  }
  .header{
    width: 100%;
    min-height: 300px;
    background: no-repeat url('./assets/images/bg-desktop-light.jpg');
    background-size: cover;
    transition: background .2s ease-in;
  }
  .dark_mode .header{
    width: 100%;
    min-height: 300px;
    background: no-repeat url('./assets/images/bg-desktop-dark.jpg');
    background-size: cover;
    
  }
  /* background corlor dark  #181824 */
  .footer {
    background: white;
    min-height: calc(100vh - 300px);
    width: 100%;
    transition: background .2s ease-in;
  }
  .dark_mode .footer {
    background: #181824;
  }
  .todo_app{
    position: absolute;
    margin: 0 auto;
    max-width: 537px;
    width: 100%;
    top: 45%;
    left: 50%;
    transform: translate(-50%,-50%);
    min-height:60vh ;
    display: flex;
    flex-direction: column;
    gap: 40px;
    padding: 0 20px;
    
  }
  .header_todo {
    min-height: 151px;
  }
  .title {
    text-transform: uppercase;
    font-weight: bold;
    font-size: 50px;
    color: white;
    display: flex;
    justify-content: space-between;
    align-items: center;
    letter-spacing: 10px;
  }
  .switch_dw {
    cursor: pointer;
    display: block;
    width: 30px;
    height:30px;
    background: no-repeat url('./assets/images/icon-moon.svg');
    background-size: cover;
    transition: background .1s ease-in;
  }
  .dark_mode .switch_dw {
    background: no-repeat url('./assets/images/icon-sun.svg');
  }
  .task {
    border-radius: 10px;
    padding: 10px;
    width: 100%;
    background: white;
    display: flex;
    gap: 15px;
    transition: background .2s ease-in;
  }
  .btn {
    cursor: pointer;
    display: block;
    width: 30px;
    height:30px;
    border: 1px solid #8299F7;
    background-color: white;
    border-radius: 50%;
    background-size: cover;

  }
  .dark_mode  .btn{
    background-color: #25273C;
  } 
  .else {
    justify-content: center;
    width: 100%;
  }
 
  .btn_done {
    background: no-repeat url('./assets/images/icon-check.svg');
    background-color: #719ECE !important;
    background-size: 50%;
    background-position: center center;
  }
  /* #25273C */
  .input {
    width: 100%;
    background: white;
    border: none;
    padding: 0 10px ;
    border-radius: 5px;
    transition: background .2s ease-in;
  }
  .dark_mode .task, .dark_mode .input {
    background:#25273C;
    color: white;
    text-decoration-color:black;

  }
  .edit_task {
    min-height: 30px;
  }
  .dark_mode .line_throu {
    text-decoration-color:#181824;
    color: rgba(243, 236, 236, 0.4);
  }
  .line_throu {
    text-decoration-line: line-through;
    text-decoration-color:#181824;
 
    color: rgba(2, 2, 2, 0.3);
  }
  input::placeholder {
    color: grey;
    text-transform: uppercase;
    font-family: 'Poppins', sans-serif;
  }
  input:focus {
    outline: none !important;
    border: solid 1px  #719ECE;
  }
  .todo_container {
    margin: 30px 0;
    box-shadow: 1px 1px 7px rgba(0,0,0,.3);
    border-radius: 5px ;
  }
  .todo_item {
    min-height: 60px;
    display: flex;
    align-items: center;
    border-bottom: 0.5px solid grey;
    border-radius: 5px 5px 0 0;
  }
  .cross_input {
    width: 15px;
    height: 15px;
    cursor: pointer;
  }
  .manage {
    min-height: 30px;
    padding: 0 10px;
    font-size: 12px;
    display: flex;
    letter-spacing: none;
    font-family: 'Poppins', sans-serif;
   
    justify-content: space-between;
    align-items: center;

  }
  .dark_mode  .manage {
    color: white;
    background: #25273C;
  }
  .manage ul{
    display: flex;
    justify-content: space-between;
    gap: 10px;
  }
  .clear_tasks {
    cursor: pointer;
  }
  .filter li {
    cursor: pointer;
    transition: scale .2 ease-in;
  }
  .filter li:hover {
    scale: 1.1;
  }
  @media screen and (min-width: 700px) {
    .btn:hover {
    background: no-repeat url('./assets/images/icon-check.svg') ;
    background-color: #719ECE;
    background-size: 50%;
    background-position: center center;
  }
  }
</style>
