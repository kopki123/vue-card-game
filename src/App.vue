<template>
  <h1>紙牌配對</h1>
  <div class="display-bar">
    <h3>可錯誤次數 : {{ mistakeCount }}</h3>
    <h3>配對副數 : {{ matchedPairCount }} / {{ cardArray.length / 2 }}</h3>
  </div> 
  <div class="grid">
    <div v-for="card in cardArray" :key="card.id">
      <Card :card='card' @filpcard='filpCard'/>
    </div>
  </div>
  <button class="restart-btn" @click="reStart">重新開始</button>
</template> 

<script>
import { computed, onMounted, ref, watch } from 'vue'
import Card from './components/Card.vue'

export default {
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardArray = ref([
      {
        id: 1,
        num: 1,
        checked: false,
        visible: false,
      },
      {
        id: 2,
        num: 1,
        checked: false,
        visible: false,
      },{
        id: 3,
        num: 2,
        checked: false,
        visible: false,
      },{
        id: 4,
        num: 2,
        checked: false,
        visible: false,
      },{
        id: 5,
        num: 3,
        checked: false,
        visible: false,
      },{
        id: 6,
        num: 3,
        checked: false,
        visible: false,
      },{
        id: 7,
        num: 4,
        checked: false,
        visible: false,
      },{
        id: 8,
        num: 4,
        checked: false,
        visible: false,
      },{
        id: 9,
        num: 5,
        checked: false,
        visible: false,
      },{
        id: 10,
        num: 5,
        checked: false,
        visible: false,
      },{
        id: 11,
        num: 6,
        checked: false,
        visible: false,
      },{
        id: 12,
        num: 6,
        checked: false,
        visible: false,
      }
    ])

    onMounted(() => {
      cardArray.value.sort(() => 0.5 - Math.random());
    }) 

    let firstCard = ref('');
    let secondCard = ref('');
    let matchedCardArray = ref([]);
    let mistakeCount = ref(10);


    const interactive = computed(() => {
      return !secondCard.value;  
    })

    const matchedPairCount = computed(() => {
      return matchedCardArray.value.length;
    })

    const filpCard = (id) => {
      if(!interactive.value) {
        return;
      }

      const newCardArray = cardArray.value.map((card) => {
        if(card.id === id) {
          return { ...card, checked: true };
        }
        return card;
      })

      cardArray.value = newCardArray;

      if(firstCard.value === '') {
        firstCard.value = cardArray.value.find((card) => card.id === id);
        return;
      }

      if(firstCard.value.id === id) {
        alert("don't click same card");
        return;
      }
      
      secondCard.value = cardArray.value.find((card) => card.id === id);
    }

    watch(secondCard, () => {
      if(secondCard.value === '') {
        return;
      }

      if(firstCard.value.num === secondCard.value.num) {
        matchedCardArray.value.push(firstCard.value.num);

        cardArray.value = cardArray.value.map((card) => {
          if(card.id === firstCard.value.id || card.id === secondCard.value.id) {
            return { ...card, visible: true };
          }
          return card;
        });

        if(matchedCardArray.value.length === cardArray.value.length / 2) {
          setTimeout(() => {
            alert('你贏了!!!');
            reStart();
          }, 10);
        }

        firstCard.value = '';
        secondCard.value = '';
      } else {
        mistakeCount.value -= 1;
        if(mistakeCount.value <= 0) {
          alert('錯太多次');
          reStart();
          return;
        }


        setTimeout(() => {
          cardArray.value = cardArray.value.map((card) => {
              if(card.id === firstCard.value.id || card.id === secondCard.value.id) {
                return { ...card, checked: false };
              }
              return card;
            });

          firstCard.value = '';
          secondCard.value = '';
        }, 1000)
      }
    })

    const reStart = () => {
      cardArray.value = cardArray.value.map(card => {
        return { ...card, checked: false, visible: false };
      })
      cardArray.value.sort(() => 0.5 - Math.random());
      firstCard.value = '';
      secondCard.value = '';
      matchedCardArray.value = [];
      mistakeCount.value = 10;
    }

    return { cardArray, filpCard, reStart, matchedPairCount, mistakeCount };
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.grid {

  width: 800px;
  margin: 60px auto;
  display: grid;
  grid-template-columns: repeat(4,1fr);
  gap: 10px;
  transform-style: preserve-3d;
  perspective: 200vh;
}

.display-bar {
  display: flex;
  justify-content: center;
  gap: 60px;
}

.restart-btn {
  padding: 15px 20px;
  border: none;
  border-radius: 5px;
  background: pink;
  color: white;
  font-weight: 700;
  font-size: 16PX;
  letter-spacing: 5px;
}

.restart-btn:hover {
  background: rgb(255, 149, 169);
}
</style>
