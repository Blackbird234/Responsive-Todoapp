<template>
    <div>
        <!-- array build -->
        <li v-for="(task, index) in notfinished" :key="task.id"  class="taskcard" :class="({'complete-taskcard': task.done})">
  
          <!-- buttons für erledigt, inklusive logo -->
          <button type="button" id="task-button1" @click="exectuteFinishedClick(task.id)" :class="({'complete': task.done})" @mouseover="showButton1" @mouseleave="hideButton1">
            <div></div>
            <Transition name="button1">
            <img src="./icons/check.svg" id="checkmark" v-if="hoverButton1">
            </Transition>
          </button>
  
          <!-- Array  description -->
          <p>{{ task.description }}</p>
  
          <!-- buttons fürlöschen, inklusive logo -->
          <button type="button" id="task-button2" @mouseover="showButton2" @mouseleave="hideButton2" @click="executeDeleteClick(index),!hideButton2">
            <div></div>
            <Transition name="button2">
            <img src="./icons/remove.svg" id="remove" v-if="hoverButton2">
          </Transition>
          </button>
        </li>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      // Export props um an Hauptkomponente zu pushen
      notfinished: Array,
      executeDeleteClick: Function,
      exectuteFinishedClick: Function,
    },
    data() {
      return {
        switchToComplete: false,
        hoverButton1: false,
        hoverButton2: false,
      }
    },
    methods: {
        showButton2() {
          this.hoverButton2 = true
        },
        hideButton2 () {
          this.hoverButton2 = false
        },
        showButton1() {
          this.hoverButton1 = true
        },
        hideButton1 () {
          this.hoverButton1 = false
        }, 
  
        // delete click eingabe push um in der hauptkomponente darauf zu zu greifen
        deleteclick(index) {
          this.executeDeleteClick(index);
        },
    }
  }
  
  
  
  </script>