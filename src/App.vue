<script setup>
  import CameraComponent from './components/CameraComponent.vue';
  import upload from './components/upload.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.png"  /> 
  
  </header>

  <main>
    <upload />
    <CameraComponent/>

  </main>

<dim> 
  <br><br><br>
    <h1>
    <RouterView></RouterView>
    
  </h1>
  </dim>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

h1 {text-align: center;}
</style>
