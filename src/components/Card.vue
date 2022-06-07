<template>
    <div id="app">
        <div  v-if="card.visible" class="card-face">
            <img 
             :src="card.img"
             :class="cardMatched"
            > 
        </div>
        <div v-else
            class="card-back"
            @click="emitCard()"
            >
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            card: {
                type: Object, 
            },
            click: {
                type: Number
            }
        },
    computed: {
      //Classes
        cardMatched: function() {
            return {
            matched: this.card.matched
        }
      },
        cardTurned: function() {
            return {
            cardBack: this.card.visible
        }
    },    
    },       
        data: () => {
            return {
               selectedCards: []       
            } 
        },
        methods:  {
            emitCard() {
                this.$emit('select', this.card);
                
            }
        }
    }
</script>