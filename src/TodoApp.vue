<template>

    <input  class="form-control"
            type="text"
            v-model="searchTxt"
            placeholder="Search..."
            />

    <TodoForm @add-todo="addTodo"  />

    <div style="color :red ;">{{ error }}</div>

    <TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="onDelete" />
    
</template>

<script>

import { ref , computed } from 'vue';
import TodoForm from './components/todo/TodoForm.vue' ;
import TodoList from './components/todo/TodoList.vue' ;
import axios from 'axios' ;


export default {

    components : {
        TodoForm,
        TodoList,

    },

    setup() {
        const todo = ref('');
        const todos = ref([]);
        const error = ref('');
        const searchTxt = ref('');
        /*
        const toggleTodo = (index) => {
            todos.value[index].completed = !todos.value[index].completed;
        };
        */

        const toggleTodo = async (index) => {
            error.value = '';
            
            
            const key =todos.value[index].id ;
            const updateData = {
                completed : !todos.value[index].completed
            };
            try{
                const response = await axios.patch(`http://localhost:3000/todos/${key}`, updateData)
                console.log(response.data);
                todos.value[index].completed = response.data.completed
            } catch(err){
                console.log(err);
                error.value = 'Something went wrong' ;
            }
        }

        const getTodos = async () => {
            error.value = '';
            try{
                const response = await axios.get("http://localhost:3000/todos");
                console.log(response.data);
                todos.value = response.data ;
            } catch (err) {
                console.log(err);
                error.value = 'Something went wrong !';
            }
        }

        getTodos();

        const filteredTodos = computed(() => {
            if (searchTxt.value) {
                return todos.value.filter(todo => {
                    return todo.subject.includes(searchTxt.value);
                });
            }

            return todos.value;
        })


        const onDelete = async (index) => {
            console.log("debug >>> todos delete index, " , index);
            error.value = '';
            const id = todos.value[index].id;
            try{
                const response = await axios.delete(`http://localhost:3000/todos/${id}`);
                console.log("debug >>> todos delete response , ", response);
                todos.value.splice(index,1);
            } catch(err){
                console.log(err);
                error.value = 'Something went wrong !';
            }
        }


        /*
        const onDelete = (index) => {
            console.log("debug >>> todos delete index", index);
            todos.value.splice(index,1);

        }
        */



        /*
        const addTodo = (data) => {
            todos.value.push(data);
        }
        */
        /*
        const addTodo = (data) => {
            error.value='';
            console.log("debug >>> addTodo data ", data);
            console.log("debug >>> addTodo data", data.subject);
            axios.post('http://localhost:3000/todos', {
                id : Date.now(),
                subject : data.subject,
                completed : data.completed
            }).then(response => {
                console.log(response);
                console.log(response.data);
                todos.value.push(response.data);
            }).catch(err => {
                console.log(err);
                error.value = 'Something went wrong !';
            });
        }
        */

        const addTodo = async (data) => {
            try{
                const response= await axios.post('http://localhost:3000/todos',{
                    id : Date.now(),
                    subject : data.subject,
                    completed : data.completed
                }) ;
                todos.value.push(response.data);
            } catch(err){
                console.log(err);
                error.value = 'Something went wrong !';
            }
        }

        return{
            todo,
            todos,
            onDelete,
            searchTxt,  
            filteredTodos,
            addTodo,
            toggleTodo,
            error,


        }   
    
    
    },
}
</script>

<style>


</style>