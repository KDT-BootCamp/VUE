<template>
    
    <div class="container">
        <h2>Todo Read View</h2>  
    
        <div v-if="loading">
            Loading.....
        </div>
        <form v-else @submit.prevent="saveTodos">
        <div class="row">
            <div class="col-6">
                <div calss="form-group">
                    <label>Todo Subject</label>
                    <input type="text" class="form-control" v-model="todo.subject" />
                </div>
            </div>
            <div class="col-6">
                <div class="form-group">
                    <label>State</label>
                    <div>
                        <button type="button" class="btn" 
                        :class="todo.completed ? 'btn-primary' : 'btn-danger'"
                        @click="checkCompleted">
                            {{ todo.completed ? "Incompeted" : "uncompleted" }}</button>                    
                    </div>
                </div>
            </div>
        </div>

        <div class="col=12">
            <div class="form-group">
                <label>Body</label>
                <textarea v-model="todo.body" class="form-control" cols="30" rows="10"></textarea>
            </div>
        </div>

        <button class="btn btn-outline-dark ml-2" :disabled="!todoUpdated">Save</button>
        <button class="btn btn-outline-danger ml-2" @click="moveToView">Cancel</button>

        </form>
        <transition name="fade">
        <Alert v-if="showAlert" :message="alertMsg" :type:="alertType"/>
        </transition>
    </div>
    
</template>

<script>

import { ref , computed } from 'vue';
import axios from '@/axios' ;
import { useRouter, useRoute } from 'vue-router' ;
import _ from 'lodash' ;
import Alert from '@/components/alert/AlertComponent.vue' ;
import { useToast } from '@/composables/toast' ;


export default {

    components : {
        Alert,

    },

    setup() {
            const route = useRoute();
            const router = useRouter() ;
            const loading = ref(true);
            const todo = ref(null) ;
            const originalTodo = ref(null);
            /*const showAlert = ref(false);
            const alertMsg = ref('');
            const alertType = ref('');
            */
            console.log("debug >>> TodoView, " , route.params);        
            console.log("debug >>> TodoView, " , route.params.id);        

            const getTodos = async () => {
               try{
                    const response = await axios.get(`todos/${route.params.id}`);
                    console.log(response.data);
                    todo.value = {...response.data} ;
                    originalTodo.value = {...response.data} ;                    
                    loading.value = false;
                    triggerAlert('Successfully get Todo...');

                } catch (err) {
                    console.log(err);
                    triggerAlert('Something went wrong...','danger');
                }   
            }

            getTodos();
            
            const {

                showAlert,  alertMsg, alertType, triggerAlert

            } = useToast();


            //endPoint가 뭔데
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
            const todoUpdated = computed(() => {
                return !_.isEqual(todo.value,originalTodo.value) ;
            }) ;

            const checkCompleted = () => {
                todo.value.completed = !todo.value.completed;
            }

            const moveToView = () => {
            //    router.push('/todos');
                router.push({
                    name : 'Todos',
                
                });
            }

            const saveTodos = async () => {
               try{
                    const response = await axios.put(`todos/${route.params.id}`,todo.value);
                    console.log("response.data >>> ",response.data);
                    console.log("todo.value >>> " , todo.value);
                    moveToView();
                    // triggerAlert('Sucessfully Save !!');

                }
                catch (err) {
                    console.log(err);
                    triggerAlert('Something went wrong...','danger');

                }   
            }


            return {
                loading,
                todo,
                moveToView,
                checkCompleted,
                saveTodos,
                todoUpdated,
                originalTodo,
                showAlert,
                alertMsg,
                alertType,
                

            }
        
        },
}
</script>

<style>
    .fade-enter-active,
    .fade-leave-active {
        transition : all 0.5s ease ;
    }
    .fade-enter-from
    .fade-leave-to {
        opacity : 1 ;
        transform : translateY(-30px) ;
    }
    .fade-enter-to
    .fade-leave-from {
        opacity : 1 ;
        transform : translateY(-30px) ;
    }
</style>