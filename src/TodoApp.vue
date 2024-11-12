<template>
    <div class="container">
        <h2>TO-Do List Page</h2>

        <TodoSearchTxt @search-todo="filteredTodos" />

        <TodoSimpleForm @add-todo="addTodo" />

        <div v-if="!todos.length" style="color: red;">
            추가된 일정이 없습니다
        </div>

        <TodoList :todos="filterTodos" @toggle-todo="toggleTodo" @delete-todo="onDelete"/>

    </div>
</template>

<script>
import { ref, computed } from 'vue';
import TodoList from './components/TodoList.vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue' ;
import TodoSearchTxt from './components/TodoSearchTxt.vue';

export default {
    components : {
        TodoSimpleForm,
        TodoList,
        TodoSearchTxt,
    }, 

    setup() {
        const todos = ref([]);
        const search = ref('');
        const filteredTodos = (searchTxt) => {
            search.value = searchTxt.value;
            console.log("debug >>> computed , " , search.value);
        };

        const filterTodos = computed(() => {
            if(search.value){
                return todos.value.filter(todo => {
                    return todo.subject.includes(search.value);
                })
            }
            return todos.value;
        })

        const addTodo = (data) => {
            console.log("debug >>> form emit data addTodo, ", data);
            todos.value.push(data);
        }

        const onDelete = (index) => {
                console.log("debug >>> delete btn click index = ", index);
                todos.value.splice(index,1);
        }

        const toggleTodo = (index) => {
            console.log("debug >>> toggleTodo index, ", index);
            console.log("debug >>> toggleTodo before, ", todos.value[index]);
            todos.value[index].completed =!todos.value[index].completed;
            console.log("debug >>> toggleTodo after, ", todos.value[index]);
        }

        return{
            todos,
            addTodo,
            onDelete,
            toggleTodo,
            filteredTodos,
            filterTodos,
        }

    },
}
</script>

<style>
</style>