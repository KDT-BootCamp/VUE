<template>
    <div class="container">
    <h5>Login View</h5>
    <form @submit.prevent="onLogin">
        <div class="mb-3">
            <label for="exampleFormControlInput1" class="form-label">ID</label>
            <input type="text" class="form-control" id="exampleFormControlInput1" 
            placeholder="ID" v-model="id">
        </div>
        <div class="mb-3">
            <label for="exampleFormControlInput2" class="form-label">PASSWORD</label>
            <input type="text" class="form-control" id="exampleFormControlInput1" 
            placeholder="PASSWORD" v-model="pw">
        </div>
        <div class="d-grid gap-2 mx-auto" align="center">
            <button class="btn btn-primary">Login</button>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            <button class="btn btn-primary" type="button" @click="returnHome">Cancel</button>
        </div>
    </form>
    </div>
</template>

<script>
import axios from '@/axios' ;
import { useRouter } from 'vue-router' ;
import { ref } from 'vue' ;
export default {
    setup() {
        
        const router = useRouter() ;
        const id = ref('');
        const pw = ref('');
        const onLogin = async () => {
            console.log("debug >>> login ");
            try{

                const data = {
                    id : id.value,
                    password : pw.value
                }

                let response = await axios.get(`users/${data.id}`,{
                    params : {
                        password : data.password
                    }
                });
                console.log(response.data);
                if(response.data.length > 0 ) {   //response.data 길이가 0 이상이면 true
                    router.push({
                        name : 'Todos'
                    })
                }
            } catch(err) {
                console.log(err) ;
            }
        }

        const returnHome = async () => {
                router.push({
                    name : 'Home'
                })
        }

    return {
        returnHome,
        id,
        pw,
        onLogin,
        
    }

    },
}
</script>
