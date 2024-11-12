<template>
  <nav class="navbar navbar-expand-lg bg-body-tertiary">
  <div class="container-fluid" style="background-color: ghostwhite;">
    <a class="navbar-brand" href="#">KDT-MultiCampus</a>
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <router-link class="nav-link active" aria-current="page" :to="{ name : 'Home' }">Home</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link active" aria-current="page" :to="{ name : 'Todos' }">Todos</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link active" aria-current="page" :to="{ name : 'Login' }">Login</router-link>
        </li>
      </ul>
  </div>
</nav>
<router-view></router-view>
</template>

<script>

export default {
  setup(){

  },
}
</script>

<style>

</style>
