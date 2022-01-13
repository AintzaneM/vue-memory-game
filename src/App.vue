<template>
  <h1>Memory Game</h1>
  <section class="board">
    <Card
      v-for="(card, index) in cardList"
      :key="`card-${index}`"
      :value="card.value"
      :visible="card.visible"
      :position="card.position"
      :matched="card.matched"
      @select-card="flipCard"
    />
  </section>

  <h2>{{ status }}</h2>
  <h2 id="timer"></h2>
  <button @click="reloadGame">Restart</button>
</template>

<script>
import _ from "lodash";
import { computed, ref, watch } from "vue";
import Card from "./components/Card.vue";

export default {
  name: "App",
  components: {
    Card,
  },
  setup() {
    const cardList = ref([]);
    const userSelect = ref([]);
    const status = computed(() => {
      if (cardTotal.value === 12) {
        return "Player wins!!!";
      } else {
        return `Card turns: ${cardTotal.value}`;
      }
    });
    const cardTotal = computed(() => {
      const cardFiltered = cardList.value.filter(
        (card) => card.visible === true
      ).length;
      return cardFiltered;
    });
    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value);
    };
    const time = () => {
      let secs = 0;
      let mins = 0;
      let SS;
      let MM;

      setInterval(() => {
        secs++;
        if (secs === 60) {
          secs = 0;
          mins++;
        }

        secs < 10 ? (SS = `0 ${secs}`) : (SS = `${secs}`);
        mins < 10 ? (MM = `0${mins}`) : (SS = `${mins}`);

        if (cardTotal.value === 12) {
          return `${MM}:${SS}`;
        }
        document.querySelector("#timer").innerHTML = `${MM}:${SS}`;
      }, 1000);
    };
    const reloadGame = () => {
      shuffleCards();
      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          matched: false,
          visible: false,
          position: index,
        };
      });
    };
    const cardItems = ["ada", "heidy", "jocelyn", "lise", "marie", "mary"];
    cardItems.forEach((item) => {
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false,
      });
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false,
      });
    });
    cardList.value = cardList.value.map((card, index) => {
      return {
        ...card,
        position: index,
      };
    });
    const flipCard = (data) => {
      cardList.value[data.position].visible = true;
      if (userSelect.value[0]) {
        if (
          userSelect.value[0].position === data.position &&
          userSelect.value[0] === data.faceCardValue
        ) {
          return;
        } else {
          userSelect.value[1] = data;
        }
      } else {
        userSelect.value[0] = data;
      }
    };
    watch(
      userSelect,
      (currentValue) => {
        if (currentValue.length === 2) {
          const cardFirst = currentValue[0];
          const cardSecond = currentValue[1];
          if (cardFirst.faceCardValue === cardSecond.faceCardValue) {
            cardList.value[cardFirst.position].matched = true;
            cardList.value[cardSecond.position].matched = true;
          } else {
            setTimeout(() => {
              cardList.value[cardFirst.position].visible = false;
              cardList.value[cardSecond.position].visible = false;
            }, 2000);
          }
          userSelect.value.length = 0;
        }
      },
      { deep: true }
    );

    return {
      cardList,
      flipCard,
      userSelect,
      status,
      cardTotal,
      shuffleCards,
      reloadGame,
      time,
    };
  },
};
</script>

<style>
@import "./assets/App.css";
</style>
