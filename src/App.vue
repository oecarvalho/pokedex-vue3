<template>
  <Header/>

  <div class="container">
    <main>
      <MenuLateral/>
      <div class="principal">
        <Buscar/>
        <Card/>
      </div>
    
    </main>
  </div>
  
  
</template>



<script>
  import Header from './components/Header.vue';
  import MenuLateral from './components/MenuLateral.vue';
  import Buscar from './components/Buscar.vue';
  import Card from './components/Card.vue';

  export default {
    name: 'App',
    components: {
      Header,
      MenuLateral,
      Buscar,
      Card
    }
  }
</script>



<style>
 
  main{
    width: 100%;
    display: flex;
    gap: 4rem;
  }

  .principal{
    width: 100%;
    max-width: 1050px;
  }

</style>
