<template>
    <div class="container">
        <h2>Todos Page</h2>

        <input  class="form-control"
            type="text"
            v-model="searchTxt"
            placeholder="Search..."
            />
    
        <!----<TodoForm @add-todo="addTodo"  />--->

        <div class="d-grid gap-2" align="right">
            <button class="btn btn-primary" type="button" @click="createPage">Create</button>
        </div>


        <div style="color :red ;">{{ error }}</div>

        <TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @open-modal="openModal"/>

        <Alert  v-if="showAlert" :message="alertMsg" :type:="alertType"/>

        <Teleport to="#modal">
            <Modal v-if="showModal">
                <template v-slot:title>
                    할 일 삭제하기
                </template>
                <template v-slot:body>
                    증말로 삭제할건강 ?
                </template>
                <template v-slot:footer>
                    <button type="button" class="btn btn-secondary" @click="onClose">Close</button>
                    <button type="button" class="btn btn-primary" @click="deleteTodo">Delete</button>
                </template> 
            </Modal>
        </Teleport>
    </div>    
</template>

<script>
import { ref , computed } from 'vue';
//import TodoForm from '@/components/todo/TodoForm.vue' ;
import TodoList from '@/components/todo/TodoList.vue' ;
import axios from '@/axios' ;
import Alert from '@/components/alert/AlertComponent.vue' ;
import { useToast } from '@/composables/toast' ;
import { useRouter, /*useRoute*/ } from 'vue-router' ;
import Modal from '@/components/modal/ModalComponent.vue' ;

export default {

    components : {
        //TodoForm,
        TodoList,
        Alert,
        Modal,
    },

    setup() {
        const todo = ref('');
        const todos = ref([]);
        const error = ref('');
        const searchTxt = ref('');
        //const route = useRoute();
        const router = useRouter() ;
        const showModal = ref(false);
        let deleteTodoId ;

        /*
        const showAlert = ref(false);
        const alertMsg = ref('');
        const alertType = ref('');
        */

        /*
        const toggleTodo = (index) => {
            todos.value[index].completed = !todos.value[index].completed;
        };
        */

       const deleteTodo = async () => {
            console.log("debug >>> todos delete index, " , deleteTodoId);
            try{
                const response = await axios.delete(`todos/${deleteTodoId}`);
                console.log("debug >>> todos delete response , ", response);
                getTodos();
                showModal.value = false;
            } catch(err){
                console.log(err);
            }
        }

        const onClose = () => {
            console.log("debug  >>> modal") ;
            showModal.value = false;
        }

        const openModal = (id) => {
            console.log("debug >>> open modal ") ;
            deleteTodoId = id ;
            showModal.value = true ;
        }


        const toggleTodo = async (index) => {
            error.value = '';
            
            
            const key =todos.value[index].id ;
            const updateData = {
                completed : !todos.value[index].completed
            };
            try{
                const response = await axios.patch(`todos/${key}`, updateData)
                console.log(response.data);
                todos.value[index].completed = response.data.completed
                triggerAlert('Success Chceck toggle !!','primary');
            } catch(err){
                console.log(err);
                error.value = 'Something went wrong' ;
            }
        }

        const getTodos = async () => {
            error.value = '';
            try{
                const response = await axios.get("todos");
                console.log(response.data);
                todos.value = response.data ;
            } catch (err) {
                console.log(err);
                error.value = 'Something went wrong !';
            }
        }

        getTodos();


        const createPage = () => {
            router.push({
                    name : 'Create',
                });
        }


        const {

            showAlert,  alertMsg, alertType, triggerAlert

        } = useToast();

        /*
        const triggerAlert = (message, type = 'success') => {
                showAlert.value = true ;
                alertMsg.value = message ;
                alertType.value = type ;
                setTimeout(() => {
                    showAlert.value = false ;
                    alertMsg.value = '' ;
                    alertType.value = '' ;
                },3000) ;
            }
        */
        const filteredTodos = computed(() => {
            if (searchTxt.value) {
                return todos.value.filter(todo => {
                    return todo.subject.includes(searchTxt.value);
                });
            }

            return todos.value;
        })

        /*
        const onDelete = async (index) => {
            console.log("debug >>> todos delete index, " , index);
            error.value = '';
            const id = todos.value[index].id;
            try{
                const response = await axios.delete(`todos/${id}`);
                console.log("debug >>> todos delete response , ", response);
                todos.value.splice(index,1);
            } catch(err){
                console.log(err);
                error.value = 'Something went wrong !';
            }
        }
        */

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
        /*
        const addTodo = async (data) => {
            try{
                const response= await axios.post('todos',{
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
        */
        return{
            todo,
            todos,
            //onDelete,
            searchTxt,  
            filteredTodos,
            toggleTodo,
            error,
            alertMsg,
            alertType,
            showAlert,
            createPage,
            onClose,
            showModal,
            openModal,
            deleteTodo,

        }   
    
    
    },
}
</script>


<style>
</style>