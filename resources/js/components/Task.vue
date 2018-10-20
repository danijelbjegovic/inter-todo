<template>
        <tr>
            <td>{{ task.title }}</td>
            <td>{{ task.priority }}</td>
            <td><button @click="remove" class="btn btn-danger">remove</button></td>
        </tr>

</template>

<script>
export default {
    props: ['task'],
    methods:{
        remove(){
            this.$emit('delete', this.task.id)
        },
    }
}
</script>

<style>

</style>
