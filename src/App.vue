<template>
  <Header/>

  <div class="container">

    <Card/>
  
  </div>
  
  
</template>



<script>
  import Header from './components/Header.vue';
  import Card from './components/Card.vue';

  export default {
    name: 'App',
    components: {
      Header,
      Card
    }
  }
</script>



<style scoped>

</style>
