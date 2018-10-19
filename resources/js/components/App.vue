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


                <task-component v-for="task in tasks" :key="task.id" :task="task"></task-component>


                <tr>

                    <td>
                        <input class="form-control" type="text" id="task">
                    </td>

                    <td>
                        <select name="" id="select" class="form-control">
                            <option value="">Low</option>
                            <option value="">Medium</option>
                            <option value="">High</option>
                        </select>
                    </td>
                    <td>
                        <button class="btn btn-primary">Add</button>
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
            message: 'Hello from here....'
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
