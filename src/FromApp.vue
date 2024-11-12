<template>
    <form @submit.prevent="onSubmit" class="d-flex">
        <div class="flex-grow-1 mr-2">
            <input class="form-control" type="text" placeholder="Add your To do" v-model="todo">
        </div>

        <div>
            <button class="btn btn-primary" type="submit">
                ADD
            </button>
        </div>
    </form>

    {{ todos }}
</template>

<script>
import {ref} from 'vue';

export default {
    setup() {
        const todo = ref('');
        const todos = ref([]);
        const onSubmit = () => {
            console.log("debug >>> event ");
            todos.value.push({
                id : Date.now(),
                subject : todo.value
            });
        }
    
        return{
            onSubmit,
            todos,
            todo
        }

    
    },
    
}
</script>

<style>

</style>