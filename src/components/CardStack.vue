<template>
    <div class="cardstack">     
        <Card v-for="(card, index) in cards" v-bind:key="index" class="card" 
        v-on:click.native="changeCard(index)"
        v-bind:cardInfo="card" v-show="index !== id" />
    </div>
</template>

<script>
import { bus } from '../main';
import Card from './Card'

export default {
    name: 'CardStack',
    components: {
        Card
    },
    props: {
        cards: Array,
        id: Number
    },
    methods: {
        changeCard: function (value) {
            bus.$emit('changeCard', value);
        }
    }
}
</script>

<style>
.cardstack {
    margin-top: 13rem;
    display: flex;
    flex-direction: column;
}
.cardstack > .card {
    margin-top: -12rem;
} 
</style>