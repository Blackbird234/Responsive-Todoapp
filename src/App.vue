<script>
import Tasks from './components/Tasks.vue'; 
import TasksDone from './components/TasksDone.vue';
import TasksUnfinished from './components/TasksUnfinished.vue';
import UserName from './components/UserName.vue';

  export default {
  data() {
    return {
      currentDate: null,
      TasksVisible: false,
      TasksDoneVisible: false,
      TasksUnfinishedVisible: false,
      InputVisible: false,
      newTask:"",
      tasks: [],
      finished: [],
      notfinished: [],
      lasttaskID: 0,
      showUserNameModal:true,
      userName:"",
    }
  },
  components: {
    Tasks,
    TasksDone,
    TasksUnfinished,
    UserName,
},
  mounted() {
    this.getCurrentDate();
  },
  methods: {
      UpdateUserName(userName) {
        this.userName = userName;
        this.showUserNameModal = false;
      },

      // Bezug für aktuelles Datum
      getCurrentDate() {
      const date = new Date();
      const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
      this.currentDate = date.toLocaleDateString('de-DE', options);
      },

      //button toggles 3x
      toggleTasks() {
      this.TasksVisible = !this.TasksVisible;
      },
      toggleTasksdone() {
        this.TasksDoneVisible = !this.TasksDoneVisible;
      },
      toggleTasksunfinished() {
        this.TasksUnfinishedVisible = !this.TasksUnfinishedVisible;
      },

      //toggle für mouseover funktionen 
      showinput() {
        this.InputVisible = true;
      },
      hideinput() {
        this.InputVisible = false;
      },

      // task hinzufügen über inputfeld und aufklappen des tasks array bereiches
      addtask () {
        this.tasks.push({id: this.lasttaskID++,description: this.newTask, done: false});
        this.newTask = '';
        this.TasksVisible = true;
        },

      // tasks löschen und danach leeren task container wieder schließen
      deleteTask (index) {
        this.tasks.splice(index,1);
          if (this.tasks.length === 0) {
          this.TasksVisible = false;
          }      
        },

      // array sortierung für fertig und unfertig und sichtbarkeit der einzelnen array container

      togglefinishedState(taskId) {
        const taskIndex = this.tasks.findIndex(task => task.id === taskId);
          if (taskIndex !== -1) {
        this.tasks[taskIndex].done = !this.tasks[taskIndex].done;
        const task = this.tasks[taskIndex];
          if (task.done) {
          this.finished.push({ id: task.id, description: task.description, done: true });
          } else {
          this.notfinished.push({ id: task.id, description: task.description, done: false });
        }
      }
    },
  },

  computed: {
      tasksToDo () {
        return this.tasks.filter(task => !task.done).length;
      },
      tasksFinished () {
        return this.tasks.filter(task => task.done).length;
      },
      finished() {
        return this.tasks.filter(task => task.done)
      },
      finishedNF() {
        return this.notfinished.filter(task => task.done)
      },
      notfinished() {
        return this.tasks.filter(task => !task.done)
      },
    }
  }
</script>

<template>
  <!-- Popup -->
  <UserName :showModal="showUserNameModal" @closeModal="UpdateUserName"/>

  <header>
    <!-- header bereich mit datum,mamen und logo -->
    <h1> Hallo {{ userName }},
      <p> Heute ist {{ currentDate }}</p>
    </h1>
    <img src="./components/icons/logo.svg" class="logo">

    <!-- informationsbereich zu den array statistiken -->
    <div class="header-information">
      <div class="tasklist"> 
        <p> Du hast noch <b>{{ tasksToDo }}</b> offene <span v-if="tasksToDo!==1">Aufgaben</span><span v-else>Aufgabe</span></p>
        <p v-if="tasksFinished >= 1"> <b>{{ tasksFinished }}</b> <span v-if="tasksToDo!==1">Aufgaben</span><span v-else>Aufgabe</span> bereits erledigt</p>
      </div>

      <!-- toggle buttons für die verschiedenen arrays -->
      <div class="radio-buttons">
       <div class="radio-button" :class="{'activated' : TasksVisible}" @click="toggleTasks">
          <img src="./components/icons/list.svg" class="radio-svg" id="list">
        </div>
       <div class="radio-button" :class="{'activated' : TasksDoneVisible}" @click="toggleTasksdone">
          <img src="./components/icons/check.svg" class="radio-svg" id="check">
        </div>
        <div class="radio-button" :class="{'activated' : TasksUnfinishedVisible}" @click="toggleTasksunfinished">
          <img src="./components/icons/attention.svg" class="radio-svg" id="attention">
        </div>
     </div>
    </div>    
  </header>

    <!-- ausklappbared inputfeld mit mouseover -->
   <div class="add-tasks" @mouseover="showinput" @mouseleave="hideinput">
    <Transition>
      <input v-show="InputVisible" v-model="newTask" id="input-tasks" type="text" name="text" placeholder="Füge eine Aufgabe hinzu...." @keyup.enter="addtask">
    </Transition> 
    <Transition>     
      <button v-show="InputVisible" id="input-button" @click="addtask"> Hinzufügen </button>
    </Transition>
    <Transition name="notch-opacity">
      <div id="notch" v-show="!InputVisible"></div>
    </Transition>
   </div>

     <!-- Array building section, import aus komponenten -->
    <main>
      <ul>
        <li v-if="TasksVisible" >
          <Tasks :tasks="tasks" :executeDeleteClick="deleteTask" :exectuteFinishedClick="togglefinishedState" v-model="newTask" />
        </li>
      </ul>
      <div v-if="TasksDoneVisible" class="Tasks-container" id="Tasks-container"> <p>Deine bereits erledigten Aufgaben</p></div>
      <ul>
        <li v-if="TasksDoneVisible">
          <TasksDone :finished="finished" :executeDeleteClick="deleteTask" :exectuteFinishedClick="togglefinishedState" v-model="newTask" />
        </li>
      </ul>
      <div v-if="TasksUnfinishedVisible" class="Tasks-container" id="Tasks-container"> <p>Das hast du noch <b>{{ tasksToDo }}</b> <span v-if="tasksToDo!==1">Aufgaben</span><span v-else>Aufgabe</span> vor dir </p>   </div>
      <ul>
        <li v-if="TasksUnfinishedVisible">
        <TasksUnfinished :notfinished="notfinished" :executeDeleteClick="deleteTask" :exectuteFinishedClick="togglefinishedState" v-model="newTask" />
        </li>
      </ul>
  </main>

</template>

<style scoped>
</style>
