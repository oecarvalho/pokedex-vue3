<template>
  <Header/>
  <main class="container">
    <MenuLateral/>
    <Buscar/>
  </main>
  
</template>



<script>
  import Header from './components/Header.vue';
  import MenuLateral from './components/MenuLateral.vue';
  import Buscar from './components/Buscar.vue';
  
  export default {
    name: 'App',
    components: {
      Header,
      MenuLateral,
      Buscar,
    }
  }
</script>



<style>

  main{
    display: flex;
    gap: 4rem;
  }

</style>
