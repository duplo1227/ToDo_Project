<template>
    <li>
        <button @click="$emit('complete')" :class="className" v-bind:class="changePriority()">
            <i class="far fa-circle" style="margin-right: 10px;"></i>{{ task.title }}
        </button>
        <div id="connector">
        <select v-model="task.priority" class="priority-select" style="margin-right: 10px;">
            <option value="" disabled selected>Priority</option>
            <option value="low">Low</option>
            <option value="middle">Middle</option>
            <option value="high">High</option>
        </select>
        <button @click="$emit('remove')"><i class="far fa-trash-alt"></i></button>
        </div>
    </li>
</template>
<script>
    export default{
        name: 'Taskitem',
        props: ['task'],
        computed: {
            className(){
                let classes = ['toggle'];
                if(this.task.completed){
                    classes.push('toggle-completed');
                }
                return classes;
            }
        },
        methods: {
            changePriority(){
                switch(this.task.priority){
                    case 'high':
                        return 'high-priority';
                    case 'middle':
                        return 'middle-priority';
                    default:
                        return 'low-priority';
                }
            }
        }
    }
</script>