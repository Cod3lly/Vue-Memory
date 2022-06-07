<template>
    <div id="app">
      <h1>Welcome to the memory game!</h1>
    <div class="game-board">
        <Card :card="card" v-for="(card, index) in playCards"
            :key="card.id"
            @select="selectCard"
            :value="card.value"
            :id="card.id"
         >  
         </Card>
         <h1 v-show="win"> You won with {{turns}} turns! <button @click="reloadPage">Restart!</button></h1>
         <div class="game-status">
          <div class="statusbar__value" :style="{width: statusGame + '%'}"></div>
        </div>
    </div>
    </div>
</template>
<script>
import Card from './Card.vue'
import { memoryCards } from './../data/cards'
import { pushScopeId } from '@vue/runtime-core'; 
//Note2Self: Div-Statusbar is extra graphical customisation for later.
export default {
  components: {Card},
  name: "Gameboard",
      data() {
          return {    
              cards: memoryCards,   
              playCards: [],   
              turnedCards: [],
              win: false,
              turns: 0,
              isProcessing:false,
              statusGame: 0
          };           
  },
  computed: {
    statusBarStyle() {
       return { width: this.statusGame + '%'};
    }
  },
  methods: {
      selectCard: function(card) {
          if (this.isProcessing === false) {
          card.visible = true;
          if(this.turnedCards.length < 2) {
            this.turnedCards.push(card);
          } 
          if(this.turnedCards.length === 2) {
            this.matchCards();
          }
            this.winGame();  
          } else {
            console.log("Too Soon!!!");
          }
    },
      reloadPage: function() {
      window.location.reload();
    },
      matchCards: function() {
        clearInterval(this.playTimer);
            if(this.turnedCards[0].name === this.turnedCards[1].name) {
                this.turnedCards.forEach(card => card.matched = true);
                this.turnedCards.forEach(card => card.visible = true); 
                this.turns++
                this.statusGame +=25;
      
                clearInterval(this.playTimer);
                this.turnedCards = [];
            } else {
              clearInterval(this.playTimer);
                this.isProcessing = true;
                const playTimer = setTimeout(this.flipBack, 1000);       
              } 
              },
       flipBack: function() {
                this.turns++
                this.turnedCards.forEach(card => card.visible = false);
                this.turnedCards = [];
                this.isProcessing = false;
              },
       winGame() {
            if(this.playCards.every(card => card.matched)) this.win = true;
              },
              },
  mounted() {
    //Array methods done with Lodash
                var cards1 = _.cloneDeep(this.cards);
                var cards2 = _.cloneDeep(this.cards);
                this.playCards = this.playCards.concat(cards1, cards2);
                this.playCards = _.shuffle(this.playCards);
},
  }
</script>
<style>
* {
  background-color: skyblue;
}
.game-board {
  width: 640px;
  height: 640px;
  display: flex;
  flex-wrap: wrap;
  perspective: 1000px;

}
.card-back{
  background-color: blue;
  width: 150px;
  height: 200px;
  position: relative;
  margin: 5px;
  border-radius: 5px;
  color: red;
}

.card-face {
   width: 150px;
  height: 200px;
  position: relative;
  margin: 5px;

}

img {
    width: 150px;
    height: 200px;
}

.matched{
   opacity: 0.3;
   background-color: purple;
}

#noClick {  
  pointer-events: none;  
}

h1 {
  color: white;
  margin: 30px;
  text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;
}

.game-status {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}

.statusbar__value {
  background-color: #00a876;
  width: 100%;
  height: 100%;
}

</style>