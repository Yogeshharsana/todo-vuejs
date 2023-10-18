<template>
  <div class="container">
    <div class="task">
      <!-- title -->
      <div class="title">
        <h1> To Do List</h1>
      </div>
      <!-- form -->
      <div class="form">
        <input
          type="text"
          placeholder="New Task"
          v-model="newTask"
          @keyup.enter="addTask"
        />
        <button @click="addTask"><i class="fas fa-plus"></i></button>
      </div>
      <!-- buttons -->
      <div class="clearBtns">
        <button class="dropdown-item" @click="showAllTasks">All Tasks</button>
        <button class="dropdown-item" @click="showCompletedTasks">Completed</button>
        <button class="dropdown-item" @click="showIncompleteTasks">Incomplete</button>
      </div>
      <!-- task lists -->
      <div class="taskItems">
        <ul>
          <task-item
            v-bind:task="task"
            v-for="(task, index) in filteredTasks"
            :key="task.id"
            @remove="removeTask(index)"
            @complete="completeTask(task)"
          ></task-item>
        </ul>
      </div>
      <!-- buttons -->
      <div class="clearBtns">
        <button @click="clearCompleted">Clear completed</button>
        <button @click="clearAll">Clear all</button>
      </div>
      <!-- pending task -->
      <div class="pendingTasks">
        <span>Pending Tasks: {{ incomplete }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import TaskItem from "./Task-item";

export default {
  name: "Task",
  props: ['tasks'],
  components: {
    TaskItem,
  },
  data() {
    return {
      newTask: "",
      filter: 'all',
    };
  },
  computed: {
    filteredTasks() {
      if (this.filter === 'incomplete') {
        return this.tasks.filter(task => !task.completed);
      } else if (this.filter === 'completed') {
        return this.tasks.filter(task => task.completed);
      } else   {
        
        return this.tasks;
      }
    },
    incomplete() {
      return this.tasks.filter(task => !task.completed).length;
    },
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push({
          title: this.newTask,
          completed: false,
        });
        this.newTask = "";
      }
    },
    showAllTasks() {
      this.filter = 'all';
    },
    showCompletedTasks() {
      this.filter = 'completed';
    },
    showIncompleteTasks() {
      this.filter = 'incomplete';
    },
    inProgress(task) {
      return !this.isCompleted(task);
    },
    isCompleted(task) {
      return task.completed;
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(this.inProgress);
    },
    clearAll() {
      this.tasks = [];
    },
    completeTask(task) {
      task.completed = !task.completed;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
  },
};
</script>
