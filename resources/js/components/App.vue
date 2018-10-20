<template>
    <div class="app-component">
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
                        <input v-model="task.title" class="form-control" type="text" id="task">
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
export default {
    data(){
        return{
            tasks: [],
            task: {title: '', priority: ''}
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
                axios.post('/api/tasks', this.task)
                .then(res =>{
                    this.tasks.push(res.data)
                    this.task.title = ""
                    //this.task.priority = "undefined"
                })
            }

        },
        remove(id){
            //console.log(`I got the data ${id}`)
            axios.delete(`/api/tasks/${id}`)
            .then(() =>{
                let index = this.tasks.findIndex(task => task.id === id)
                this.tasks.splice(index, 1);
            })
        }
    },
    created(){
        this.getTasks()
    },
    components:{TaskComponent}
}
</script>

<style>

</style>
