<template>
  <Header/>

  <div class="container">
    <main>
      <MenuLateral/>
      <div class="principal">
        <Buscar/>
        <div class="cards-pokemon">
          <Card/>
        </div>
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
    gap: 2rem;
  }

  .principal{
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .cards-pokemon ul{
    width: 100%;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
    justify-content: space-between;
  }
</style>
