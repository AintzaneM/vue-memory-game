<template>
  <h1>Memory Game</h1>
  <section class="board">
    <Card v-for="(card, index) in cardList" 
    
    :key="`card-${index}`" 
    :value="card.value" 
    :visible="card.visible"
    :position="card.position"
    :match="card.matched"
    @select-card="flipCard"
    />
    
  </section>

  <h2>{{status}}</h2>
  
  
</template>

<script>
import { ref, watch } from 'vue';
import Card from './components/Card.vue';

export default {
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = ref([])
    const userSelect = ref ([])
    const status = ref ("")

    

    // console.log(userSelect.value)
    
    for (let i = 0; i < 12; i++) {
      cardList.value.push({
        value: i,
        visible: false,
        position: i,
        matched: false
      })
    }

    console.log("value",cardList.value)
    console.log("cardList",cardList)


    const flipCard = payload => {
      console.log("payload", payload)

      cardList.value[payload.position].visible = true
      
      if (userSelect.value[0]) {
        userSelect.value[1] = payload
      } else {
        userSelect.value[0] = payload
      }
    }

    watch(userSelect, currentValue => {
      console.log("current", currentValue)
      
      if(currentValue.length === 2) {
        const cardFirst = currentValue[0]
        const cardSecond = currentValue[1]
         console.log("cardFirst", cardFirst)
         console.log("cardSecond", cardSecond)

         if(cardFirst.faceCardValue === cardSecond.faceCardValue) {
           status.value="Matched"
           cardList.value[cardFirst.position].matched = false
           cardList.value[cardSecond.position].matched = true
         }else {
           status.value="Not Matched"
           cardList.value[cardFirst.position].visible = false
           cardList.value[cardSecond.position].visible = false

         }

         




         


         
        console.log("That's it!")
        userSelect.value.length = 0
        console.log("userSelect",userSelect.value)
      }
    }, {deep:true})

    return {
      cardList,
      flipCard,
      userSelect,
      status,
    }

    
  }
}
</script>

<style>
@import './assets/App.css';
</style>
