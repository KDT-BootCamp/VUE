<template>
    <div v-show="toggle">true</div>
    <div v-show="!toggle">false</div>    
    <button v-on:click="onToggle">Toggle</button>
</template>

<script>
import {ref} from 'vue' ;

export default {
    setup() {
        const toggle = ref(false);
        const onToggle = () => {
            toggle.value = !toggle.value;
        }                        
    
        return{
            toggle,
            onToggle,
        }
    },
}
</script>

<style>

</style>