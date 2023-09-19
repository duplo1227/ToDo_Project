<template>
    <div class="container">
        <div class="task">
            <div class="title">
                <h1>ToDo List</h1>
            </div>
            <div class="form">
                <input type="text" placeholder="New Task" v-model="newTask" @keydown.enter="addTask">
                <button @click="addTask"><i class="fas fa-plus"></i></button>
            </div>
            <div class="taskItems">
                <ul>
                    <task-item :task="task" v-for="(task, index) in sortedTasks" :key="task.id" @remove="removeTask(index)" @complete="completeTask(task)"></task-item>
                </ul>
            </div>
            <div class="clearBtns">
                <button @click="clearCompleted">Clear completed</button>
                <button @click="clearAll">Clear all</button>
            </div>
            <div class="pendingTasks">
                <span>Pending Tasks: {{ incomplete }} </span>
            </div>
        </div>
    </div>
</template>

<script>
import TaskItem from './Task-item.vue';
    export default {
        name: 'Task',
        props: ['tasks'],
        components: {
            TaskItem
        },
        data(){
            return{
                newTask: '',
            }
        },
        computed: {
            incomplete(){
                return this.tasks.filter((task) => !task.completed).length;
            },
            sortedTasks() {
                return this.tasks.sort((taskA, taskB) => {
                    const priorityOrder = { low: 1, middle: 2, high: 3 };
                    const priorityA = priorityOrder[taskA.priority] || 0;
                    const priorityB = priorityOrder[taskB.priority] || 0;
                    if (priorityA !== priorityB) {
                        return priorityB - priorityA;
                    }
                    return taskA.id - taskB.id;
                });
            }   
        },
        methods: {
            clearAll(){
                this.$emit("clear-all");
            },
            clearCompleted(){
                const completedTasks = this.tasks.filter((task) => task.completed);
                this.$emit("clear-completed", completedTasks);
            },
            addTask(){
                if(this.newTask){
                    this.tasks.push({
                        title: this.newTask,
                        completed: false,
                        priority: ''
                    })
                    this.newTask = '';
                }
            },
            removeTask(index){
                this.tasks.splice(index, 1);
            },
            completeTask(task){
                task.completed = !task.completed;
            },
        },
    };
</script>