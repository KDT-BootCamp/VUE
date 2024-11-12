<template>
    <div>
        {{ name }}
    </div>
    <input v-bind:type="text" v-model="name"> 
    // v-bind는 양방향 데이터 통신이 가능하게 함
    <div class="container">
        <button class="btn btn-primary"
            @click="updateData">Click</button>    
    </div>
</template>

<script>

// vue 에서 이벤트 처리는 v-on:eventname="함수이름"
// data는 양방향으로 통신이 가능해야함 dom(template) <-> script
// 따라서 ref(), reactive() 함수를 이용해야 함
import {ref} from 'vue';


export default{
    setup(){
        
        let name = ref("jlism"); // 이러면 name은 반응형 data가 됨
        let type = ref('text')
        const updateData = () => {
            name.value  = "dynamic data"; // ref함수의 값을 변경하려면 value를 눌러야함
            // reactive 사용 시에는 value 없어도 값이 변경됨
            type.value = 'number';
        }

        return{
            updateData,
            name,
            type
        }
    }
}
</script>

<style>

</style>