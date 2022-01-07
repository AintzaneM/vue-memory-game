<template>
  <h1>Memory Game</h1>
  <section class="board">
    <Card v-for="(card, index) in cardList" 
    :key="`card-${index}`" 
    :value="card.value" 
    :visible="card.visible"
    :position="card.position"
    @select-card="flipCard"
    />
    
  </section>
  
  
</template>

<script>
import { ref } from 'vue';
import Card from './components/Card.vue';

export default {
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = ref([])
    for (let i = 0; i < 12; i++) {
      cardList.value.push({
        value: i,
        visible: false,
        position: i
      })
    }

    const flipCard = payload => {
      // console.log("payload", payload)
      cardList.value[payload.position].visible = true
    }

    return {
      cardList,
      flipCard
    }

    
  }
}
</script>

<style>
@import './assets/App.css';
</style>
