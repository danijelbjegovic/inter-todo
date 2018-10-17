<template>
        <tr>
            <td>{{ task.title }}</td>
            <td>{{ task.priority }}</td>
            <td><button class="btn btn-danger">remove</button></td>
        </tr>

</template>

<script>
export default {
    props: ['task']
}
</script>

<style>

</style>
