<template>
    <div class="container">생명주기 관련한 부분으로 console.log 확인 바람... </div>
    <button :disabled="flag" @click="btnHandeler">Button</button>

</template>

<script>
import { onBeforeMount, onMounted, onUpdated, onUnmounted } from 'vue';
import { ref } from 'vue' ;


export default {
    
    
    
    setup() {

        const flag = ref(false) ;

        const btnHandeler = () => {
            flag.value = true;
        }

        onBeforeMount(() => {
            console.log(">>>>> onBeforeMount");
        }); 
        onMounted(() => {
            console.log(">>>>> onMounted");
        }); 
        onUpdated(() => {
            console.log(">>>>> onUpdated");
        }); 
        onUnmounted(() => {
            console.log(">>>>> onUnMounted");
        });    
    
        return {
            btnHandeler,
            flag,

        }
    
    },
}
</script>