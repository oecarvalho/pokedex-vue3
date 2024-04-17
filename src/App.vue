<template>
  <Header/>

  <div class="container">
    <main>
      <MenuLateral/>
      <Card/>
    </main>
  </div>
  
  
</template>



<script>
  import Header from './components/Header.vue';
  import MenuLateral from './components/MenuLateral.vue';
  import Card from './components/Card.vue';

  export default {
    name: 'App',
    components: {
      Header,
      MenuLateral,
      Card
    }
  }
</script>



<style scoped>
  main{
    width: 100%;
    display: grid;
    grid-template-columns: 13.75rem 1fr;
  }
</style>
