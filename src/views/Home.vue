<template>
  <div class="home-container">
    <Top title="E-WALLET" v-bind:text="topinfo" />
    <Card v-bind:cardInfo="cardInfo" />
    <p>{{ cardStackTitle }}</p>
    <CardStack v-bind:cards="cards" v-bind:id="id" v-model="cards" />
    <button v-on:click="addCard" class="white-button">add a new card</button>
    <button v-if="show" v-on:click="removeActive">remove active card</button>
    <button v-if="show" v-on:click="removeAll" class="red-button">remove all cards</button>
  </div>
  
</template>

<script>
import { bus } from '../main';
import CardStack from '@/components/CardStack.vue'
import Card from '@/components/Card.vue'
import Top from '@/components/Top.vue'

export default {
    name: 'Home',
    components: {
      CardStack,
      Card,
      Top
    },
    data() {
        const cards = JSON.parse(window.localStorage.getItem('cardinfo'));
        let id = 0;
        let cardStackTitle = '';
        let noCards =  {
            cardNumber: 'XXXX XXXX XXXX XXXX',
            cardHolderName: 'firstname lastname',
            valid: 'MM/YY',
            ccv: '',
            vendor: 'bitcoin',
            bgColor: '#D0D0D0',
            textColor: '#000000',
            icon: require('../assets/vendor-bitcoin.svg'),
            wifi: require('../assets/chip-dark.svg')
        };
        if(cards) {
            if(cards.length > 1) {
              cardStackTitle = 'other cards';
            }
            return {
                cardInfo: {
                    cardNumber: cards[id].cardNumber,
                    cardHolderName: cards[id].cardHolderName,
                    valid: cards[id].valid,
                    ccv: cards[id].ccv,
                    vendor: cards[id].vendor,
                    bgColor: cards[id].bgColor,
                    textColor: cards[id].textColor,
                    icon: cards[id].icon,
                    wifi: cards[id].wifi
                },
                show: true,
                cards: cards, 
                id: id,
                topinfo: 'active card',
                noCards,
                cardStackTitle
            }
        }
        else {
            return {
                cardInfo: noCards,
                show: false,
                cards: cards,
                id: id,
                topinfo: 'you have no cards',
                noCards,
                cardStackTitle
            }
        }
    },
    created() {
        bus.$on('changeCard', (newId) => {
            this.id = newId;
            if(this.cards[this.id] !== undefined)
                this.cardInfo = this.cards[this.id]
        });
    },
    methods: {
        addCard: function () {
            this.$router.push({ path: 'addCard' })
        },
        removeAll: function() {
            window.localStorage.removeItem('cardinfo');
            this.cards = [],
            this.show = false,
            this.cardInfo = this.noCards,
            this.topinfo = 'you have no cards',
            this.cardStackTitle = ''
        },
        removeActive: function () {
          if (window.localStorage.getItem('cardinfo')) {
              try {
                  this.cards = JSON.parse(localStorage.getItem('cardinfo'));
                  this.cards.splice(this.id, 1);
                  window.localStorage.setItem('cardinfo', JSON.stringify(this.cards));
                  if(this.cards.length < 2) {
                      this.cardStackTitle = '';
                  }
              } catch(e) {
                  window.localStorage.removeItem('cardinfo');
              }
          }
          if(this.cards.length === 0) {
              this.removeAll();
          }
          else {
              this.cards = JSON.parse(localStorage.getItem('cardinfo'));
              this.id = 0;
              this.cardInfo = this.cards[0];
          }
        }
    }
}
</script>
<style scoped>
.home-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 1rem 0;
}
.white-button {
    background: #fff;
    border: 1px solid #000;
    color: #000;
}
.red-button {
    background: rgb(228, 54, 54);
    margin-top: 1rem;
}
p {
    font-family: 'Source Sans Pro', sans-serif;
    font-weight: 600;
    color: rgba(34, 34, 34, 0.6);
    font-size: 0.75em;
    margin: 3em 0 0 0;
}
</style>