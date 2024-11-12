<template>
  <div class="name">{{ msg }}</div>
  <div class="name">{{ greeting() }}</div>
  <div class="name">{{ hello("jlism") }}</div>
</template>

<script>
  export default{
    setup() {
      const msg ="즐거운 점심시간~~ 뭐 먹지 ?";

      const greeting = () => {
        return"hi~~" ;
      }

      const hello = (name) => {
        return`hello ${name}`;

      }

      return{
        msg,
        greeting,
        hello
        
      }

    }
  }

</script>

<style>
  .name{
    color: red;
  }
</style>



