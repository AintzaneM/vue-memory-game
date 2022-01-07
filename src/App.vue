<template>
  <h1>Memory Game</h1>
  <section class="board">
    <Card v-for="(card, index) in cardList" 
    :key="`card-${index}`" 
    :value="card" />
    
  </section>
  
  
</template>

<script>

import Card from './components/Card.vue';

export default {
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = []
    for (let i = 0; i < 12; i++) {
      cardList.push(i)
    }
    return {
      cardList
    }
  }
}
</script>

<style>
@import './assets/App.css';
</style>
