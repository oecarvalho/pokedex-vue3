<template>
  <Header/>
  <MenuLateral/>
  <Buscar/>
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

</style>
