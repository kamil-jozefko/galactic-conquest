<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/game">game</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  background: #ccc;
  border-radius: 30px;
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

header a {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 30px;
  display: inline-block;
  padding: 5px 40px;
  border-left: 1px solid var(--color-border);
  text-decoration: unset;
  color: #000;
}
</style>
