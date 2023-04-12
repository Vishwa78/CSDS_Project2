<template>
    <v-btn color="error" @click="delTask"> <v-icon> {{'mdi-close-circle'}}</v-icon> Delete </v-btn>
</template>
    <script>
    
    export default {
        props: {
            tasks: Array,
            del_title: String,
        },
        methods: {
            delTask() {
                const delete_title = this.del_title
                this.$emit('task-deleted', delete_title)
            }
        }
    }
    </script>
    