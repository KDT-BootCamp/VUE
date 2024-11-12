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
    </form>

    
        <div v-if="!todos.length" style="color: red;">
            추가된 일정이 없습니다
        </div>

        <div v-show="hasError" style="color: red;">
            This field cannot be empty !!!
        </div>

        <div  v-for="(data,index) in todos" :key="data.id" class="card mt-2">
            <div class="card-body p-2 d-flex align-items-center">
                <div class="form-check flex-grow-1">
                    <input type="checkbox" class="form-check-input" v-model="data.completed">
                    <label class="from-check-label" :style="data.completed ? todoStyle : {}">
                        {{ data.subject }}
                    </label>
                </div>

                <div>
                    <button class="btn btn-danger btn-sm" @click="onDelete(index)">Delete</button>
                </div>


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
        const todos = ref([]);


        const todoStyle = {
            textDecoration : 'line-through',
            color : 'gray'

        }

        const onSubmit = () => {
            
            hasError.value = false;
            
            if(todo.value === ''){
                hasError.value = true;
            }
            else{
                todos.value.push({
                    id : cnt++,
                    subject : todo.value,
                    completed : false
              });
              todo.value = '';
              hasError.value = false;
            }
        }

            const onDelete = (index) => {
                console.log("debug >>> delete btn click index = ", index);
                todos.value.splice(index,1);
            }

        


        return{
            todos,
            todo,
            onSubmit,
            cnt,
            hasError,
            todoStyle,
            onDelete
        }
    }
}
</script>

<style>
    .todo{
        color : gray;
        text-decoration: line-through;
    }
</style>