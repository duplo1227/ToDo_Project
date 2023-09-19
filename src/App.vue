<template>
  <div id="app">
    <task :tasks="tasks" @clear-all="clearAll" @clear-completed="clearCompleted"></task>
  </div>
</template>

<script>
  import Task from "./components/Task.vue";

  export default {
    name: 'App',
    components: {
      Task,
    },
    data(){
      return{
        tasks: [],
      };
    },
    methods: {
      clearAll(){
        this.tasks = [];
      },
      clearCompleted(completedTasks){
        this.tasks = this.tasks.filter((task) => !task.completed);
      }
    },
  };
</script>
