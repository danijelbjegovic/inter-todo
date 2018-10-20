<template>
    <div class="app-component">
        <loading :active.sync="isLoading" 
        :is-full-page="fullPage"></loading>

        <table class="table">
            <thead>
                <tr>
                    <th>Task Title</th>
                    <th>Priority</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>


                <task-component 
                v-for="task in tasks" 
                :key="task.id" 
                :task="task" 
                @delete="remove"
                ></task-component>


                <tr>

                    <td>
                        <input v-model="task.title" class="form-control" type="text" placeholder="Task title" id="task">
                    </td>

                    <td>
                        <select v-model="task.priority" name="" id="select" class="form-control">
                            <option value="" selected disabled hidden>Choose here</option>
                            <option>Low</option>
                            <option>Medium</option>
                            <option>High</option>
                        </select>
                    </td>
                    <td>
                        <button @click="store" class="btn btn-primary">Add</button>
                    </td>
                </tr>
            </tbody>

        </table>
    </div>
</template>

<script>
import TaskComponent from './Task.vue'
// Import component
import Loading from 'vue-loading-overlay';
// Import stylesheet
import 'vue-loading-overlay/dist/vue-loading.css';

export default {
    data(){
        return{
            tasks: [],
            task: {title: '', priority: ''},
            isLoading: false,
            fullPage: true
        }
    },
    methods: {
        getTasks(){
            axios.get('/api/tasks')
            .then(res => {
                this.tasks = res.data
            })
            .catch(err => {
                console.log(res)
            })
        },
        checkInputs(){
            if(this.task.title && this.task.priority){
                return true
            }
        },
        store(){
            if(this.checkInputs()){
                this.isLoading = true;
                axios.post('/api/tasks', this.task)
                .then(res =>{
                    this.tasks.push(res.data)
                    this.task.title = ""
                    //this.task.priority = "undefined"
                    this.isLoading = false;
                })
            }

        },
        remove(id){
            //console.log(`I got the data ${id}`)
            this.isLoading = true
            axios.delete(`/api/tasks/${id}`)
            .then(() =>{
                let index = this.tasks.findIndex(task => task.id === id)
                this.tasks.splice(index, 1);
                this.isLoading = false
            })
        }
    },
    created(){
        this.getTasks()
    },
    components:{TaskComponent, Loading}
}
</script>

<style>

</style>
