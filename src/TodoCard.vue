<template>
    <div class="container">
        <h2>List</h2>
        <form @submit.prevent="onSubmit" class="d-flex">
        <div class="flex-grow-1 mr-2">
            <input class="form-control" type="text" placeholder="Add your To do" v-model="todo">
        </div>

        <div>
            <button class="btn btn-primary" type="submit">
                ADD
            </button>
            
        </div>

        <div v-show="hasError" style="color: red;">
            This field cannot be empty !!!
        </div>


    </form>

        <div  v-for="todo in todos" :key="todo.id" class="card mt-2">
            <div class="card-body p-2">
             {{ todo.subject }}
            </div>
        </div>

    </div>    
</template>

<script>
import {ref} from 'vue';

export default {
    setup() {

        let hasError = ref(false);
        let cnt = 1;
        const todo = ref('');
        const todos = ref([ 
            {id : cnt++, subject : '파일럿 영화보기'},
            {id : cnt++, subject : '충분히 쉬기'}
        ]);
    
        const onSubmit = () => {
            
            hasError.value = false;
            
            if(todo.value ==''){
                hasError.value = true;
            }
            else{
                todos.value.push({
                    id : cnt++,
                    subject : todo.value
              });
              todo.value = '';
              hasError.value = false;
            }

        }


        return{
            todos,
            todo,
            onSubmit,
            cnt,
            hasError
        }
    },
}
</script>

<style>
</style>