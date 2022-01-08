<template>
  <h1>Memory Game</h1>
  <section class="board">
    <Card v-for="(card, index) in cardList" 
    
    :key="`card-${index}`" 
    :value="card.value" 
    :visible="card.visible"
    :position="card.position"
    :matched="card.matched"
    @select-card="flipCard"
    />
    
  </section>

  <h2>{{status}}</h2>
  
  <button @clilck="shuffleCards">Shuffle</button>
  <!-- <h2>Remain Cards: {{cardTotal}}</h2> -->
  
  
</template>

<script>
// import VueLodash from 'vue-lodash';
import _ from 'lodash';
// import shuffle from 'lodash/shuffle'
import { computed, ref, watch } from 'vue';
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
    
    // const status = computed(() => {
    //   if(cardTotal.value === 0) {
    //     return 'Player wins!!!'
    //   } else {
    //     return `Remain Cards: ${cardTotal.value}`
    //   }
    // })
   

    const cardTotal = computed(() => {
      const cardFiltered = cardList.value.filter (card => card.visible === false).length
      return cardFiltered
    })
    
    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value)
    }

    // console.log(userSelect.value)
    
    for (let i = 0; i < 12; i++) {
      cardList.value.push({
        value: 10,
        visible: false,
        position: i,
        matched: false
      })
    }

    // console.log("value",cardList.value)
    // console.log("cardList",cardList)


    const flipCard = data => {
      console.log("data", data)

      cardList.value[data.position].visible = true
      
      if (userSelect.value[0]) {
        userSelect.value[1] = data
      } else {
        userSelect.value[0] = data
      }
    }

    watch(userSelect, currentValue => {
      // console.log("current", currentValue)
      
      if(currentValue.length === 2) {
        const cardFirst = currentValue[0]
        const cardSecond = currentValue[1]
        //  console.log("cardFirst", cardFirst)
        //  console.log("cardSecond", cardSecond)

         if(cardFirst.faceCardValue === cardSecond.faceCardValue) {
           status.value="Matched"
           cardList.value[cardFirst.position].matched = true
           cardList.value[cardSecond.position].matched = true
         }else {
           status.value="Not Matched"
           cardList.value[cardFirst.position].visible = false
           cardList.value[cardSecond.position].visible = false

         }

         




         


         
        // console.log("That's it!")
        userSelect.value.length = 0
        // console.log("userSelect",userSelect.value)
      }
    }, {deep:true})

    return {
      cardList,
      flipCard,
      userSelect,
      status,
      cardTotal,
      shuffleCards
      // cardRemain,
      // pairsRemain
    }

    
  }
}
</script>

<style>
@import './assets/App.css';
</style>
