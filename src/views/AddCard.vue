<template>
  <div class="addcard-container">
    <Top title="add a new bank card" text="new card"  />
    <Card v-bind:cardInfo="cardInfo" />
    <CardForm />
    <button v-on:click="goHome">back to e-wallet</button>
  </div>
</template>

<script>

// @ is an alias to /src
import CardForm from '@/components/CardForm.vue'
import Card from '@/components/Card.vue'
import Top from '@/components/Top.vue'

export default {
  name: 'AddCard',
  components: {
    CardForm,
    Card,
    Top
  },
  data() {
      return {
          cardInfo: {
              cardNumber: 'XXXX XXXX XXXX XXXX',
              cardHolderName: 'firstname lastname',
              valid: 'MM/YY',
              ccv: '',
              vendor: 'bitcoin',
              bgColor: '#D0D0D0',
              textColor: '#000000',
              icon: require('../assets/vendor-bitcoin.svg'),
              wifi: require('../assets/chip-dark.svg')
          }
      }
  },
  methods: {
      goHome: function () {
          this.$router.push('/')
      }
  }
}
</script>

<style scoped>
.addcard-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 1rem 0;
}
button {
  background: #fff;
  border: 1px solid #000;
  color: #000;
}
</style>