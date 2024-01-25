<template>
  <header class="container">
    <div class="wrapper">
      <nav class="d-flex">
        <RouterLink class="nav-link fs-2" to="/">Home</RouterLink>
        <RouterLink class="nav-link fs-2" to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>
<script>
import { RouterLink, RouterView } from 'vue-router'
</script>
<style lang="css">

</style>
