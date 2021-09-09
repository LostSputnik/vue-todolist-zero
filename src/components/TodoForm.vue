<template>
    <input type="text" placeholder="What needs to be done?" @keyup.enter="addTask" v-model="tempTask">
    <div class="task" v-for="task in filteredTasks" :key="task.title" :class="{ complete: task.completed }">
        <div class="actions">
            <div class="title">
                <div v-if="!task.editing" @dblclick="startEditing(task.title)">
                    <h3>{{task.title}}</h3>
                </div>
                <div v-if="task.editing" @keyup.enter="doneEditing(task.title)" @keyup.esc="cancelEditing(task.title)">
                    <input type="text" v-model="task.tempEdit">
                </div>
            </div>
            <div class="icons">
                <span @click="toggleComplete(task.title)" class="material-icons tick">done</span>
                <span @click="deleteTask(task.title)" class="material-icons">delete</span>
            </div>
        </div>
    </div>
    <div class="summary" v-if="filteredTasks.length" :class="{ allComplete: this.allComplete }">
        <div class="checkAll">
            <span @click="toggleCompleteAll" class="material-icons tick">done</span>
            Check All
        </div>
        <div class="completedCount">
            {{incompleteCount}} item(s) left
        </div>
    </div>
    <div class="filter" v-if="tasks.length">
        <Filter @filterChange="current= $event" :current="current" />
        <div class="clearCompleted" @click="clearCompleted" v-if="incompleteCount < tasks.length">
            Clear Completed
        </div>
    </div>

</template>

<script>
import Filter from './Filter.vue'

export default {
    components: {
        Filter
    },
    data() {
        return {
            tasks: [],
            tempTask: '',
            current: 'all'
        }
    },
    methods: {
        addTask() {
            let newTask = {
                title: this.tempTask,
                completed: false,
                editing: false,
                tempEdit: null,
            }
            this.tasks.push(newTask)
            this.tempTask = ''
        },
        startEditing(title){
            let task = this.tasks.find(item => item.title === title)
            task.editing = true
            task.tempEdit = task.title
        },
        doneEditing(title){
            let task = this.tasks.find(item => item.title === title)
            task.editing = false
            task.title = task.tempEdit
            task.tempEdit = null
        },
        toggleComplete(title){
            let task = this.tasks.find(item => item.title === title)
            task.completed = !task.completed
        },
        deleteTask(title){
            let task = this.tasks.find(item => item.title === title)
            let index = this.tasks.indexOf(task)
            this.tasks.splice(index, 1)
        },
        toggleCompleteAll(){
            if(this.allComplete){
                this.tasks.forEach(task => task.completed = false)
            }
            else this.tasks.forEach(task => task.completed = true )
        },
        clearCompleted(){
            let completedTasks = [];
            this.tasks.forEach(task => {
                if(task.completed){
                    completedTasks.push(task.title)
                }
            })
            completedTasks.forEach(title => {
                let index = this.tasks.findIndex(item => item.title === title)
                this.tasks.splice(index, 1)
            })
        }
    },
    computed: {
        incompleteCount(){
            return this.tasks.filter((task) => !task.completed).length
        },
        allComplete(){
            return this.incompleteCount === 0
        },
        filteredTasks() {
            if(this.current === 'completed') {
                return this.tasks.filter(task => task.completed)
            }
            else if(this.current === 'ongoing') {
                return this.tasks.filter(task => !task.completed)
            } 
            return this.tasks
        }
    }

}
</script>

<style>
.actions, .summary, .filter {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.task, .summary {
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 4px solid #e90074;
}
.material-icons {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
}
.material-icons:hover {
    color: #777;
}
.task.complete, .summary.allComplete {
    border-left: 4px solid #00ce89;
}
.task.complete .tick, .summary.allComplete .tick {
    color: #00ce89;
}

input {
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
    margin: 20px auto;
}

.clearCompleted {
    cursor: pointer;
}

</style>