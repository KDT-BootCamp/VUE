<template>
<div class="container">
    <h2> Todo-Creat </h2>
    <div v-if="loading">
            Loading.....
        </div>
        <form v-else >
        <div class="row">
            <div class="col-6">
                <div calss="form-group">
                    <label>Todo Subject</label>
                    <input type="text" class="form-control" v-model="todo.subject" />
                    <div v-if="subjectError" style="color: red">
                        {{ subjectError }}
                    </div>
                </div>
            </div>
            <div class="col-6">
                <div class="form-group">
                    <label>State</label>
                    <div>
                        <button type="button" class="btn btn-primary">
                            Incompleted </button>                    
                    </div>
                </div>
            </div>
        </div>

        <div class="col=12">
            <div class="form-group">
                <label>Body</label>
                <textarea v-model="todo.body" class="form-control" cols="30" rows="10"></textarea>
            </div>
            <div v-if="bodyError" style="color: red">
                {{ bodyError }}
            </div>
        </div>

        <button class="btn btn-outline-dark ml-2" type="button" @click.stop="modalOn">Create</button>
        <button class="btn btn-outline-danger ml-2" @click.stop="moveToView">Cancel</button>

    </form>

    <Teleport to="#modal">
        <Modal v-if="showModal">
            <template v-slot:title>
                할 일 등록하기
            </template>
            <template v-slot:body>
                증말로 등록할건강 ?
            </template>
            <template v-slot:footer>
                <button type="button" class="btn btn-secondary" @click="onClose">Close</button>
                <button type="button" class="btn btn-primary" @click="createTodo">Create</button>
            </template> 
        </Modal>
    </Teleport>S

</div>
</template>

<script>
import { ref } from 'vue' ;
import axios from '@/axios' ;
import { useRouter } from 'vue-router' ;
import Modal from '@/components/modal/ModalComponent.vue' ;

export default {
    
    component : {
        Modal,
    },

    setup() {


        const todo = ref({});
        const router = useRouter();
        const subjectError = ref('');
        const bodyError = ref('');
        const showModal = ref('false');
        
        const modalOn = () => {
            showModal.value = true;
            console.log(showModal);
        }



        const createTodo = async () => {
            subjectError.value = '' ;
            bodyError.value = '' ;  
            const data = {
                id : Date.now(),
                subject : todo.value.subject,
                completed : false,
                body : todo.value.body
            };

            try{
                if(!todo.value.subject){
                    subjectError.value = 'Subject is required' ;
                    return ;
                } else if(!todo.value.body){
                    bodyError.value = 'Body is required' ;
                    return ;
                } else {
                    await axios.post('todos',data);                    
                }
                todo.value.subject = '' ;
                todo.value.body = '' ;
                moveToView();
            } catch(err) {
                console.log(err) ;
            }
        }

        const moveToView = () => {
            router.push({ name : 'Todos'});
        }

        /*
        const addTodo = async () => {
            try{
                await axios.post('todos',{
                    id : Date.now(),
                    subject : todo.value,
                    completed : false
                }) ;
                router.push({
                        name : 'Todos'
                    });
            } catch(err){
                console.log(err);
            }
        }
        */


        return {
            todo,
            createTodo,
            moveToView, 
            subjectError,
            bodyError,
            modalOn,
            showModal,
            

        }
    },
}
</script>

<style>

</style>