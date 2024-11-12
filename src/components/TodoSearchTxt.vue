<template>
    
    <input class="form-control" type="text" v-model="searchTxt" placeholder="Search..." @input="onSearch" />

</template>

<script>
import { ref } from 'vue';
export default {

    setup(props, context) {
        const searchTxt = ref('');    
     
        const onSearch = () => {
            context.emit('search-todo', searchTxt)
        }

    return{
       searchTxt,
       onSearch,
    }

    },

}
</script>

<style>
</style>