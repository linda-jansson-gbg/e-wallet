<template>
  <section class="container" v-bind:style="{ backgroundColor: cardInfo.bgColor, color: cardInfo.textColor }">
    <div class="wifi">
        <img v-bind:src="cardInfo.wifi" alt="">
    </div>
    <div class="vendor">
        <img v-bind:src="cardInfo.icon" alt="">
    </div>
    <div class="card-number">
        <p>{{ cardInfo.cardNumber}}</p>
    </div>
    <div class="name">
        <div>
            <p>cardholder name</p>
            <p>{{ cardInfo.cardHolderName }}</p>
        </div>
        <div>
            <p>valid thru</p>
            <p>{{ cardInfo.valid }}</p>
        </div>
    </div>
  </section>
</template>

<script>
import { bus } from '../main';

export default {
    name: 'Card',
    props: {
        cardInfo: Object
    },
    created() {
        bus.$on('changeCardNumber', (data) => {
            this.cardInfo.cardNumber = data
        });
        bus.$on('changeCardName', (data) => {
            this.cardInfo.cardHolderName = data
        });
        bus.$on('changeCardValid', (data) => {
            this.cardInfo.valid = data
        });
        bus.$on('changeCardVendor', (data) => {
            this.cardInfo.vendor = data;
            this.cardInfo.icon = require('../assets/vendor-' + data + '.svg');
            this.cardInfo.textColor = '#FFFFFF';
            this.cardInfo.wifi = require('../assets/chip-light.svg')
            
            if(data === 'bitcoin') {
                this.cardInfo.bgColor = '#ffae34',
                this.cardInfo.textColor = '#000000',
                this.cardInfo.wifi = require('../assets/chip-dark.svg')
            }
            else if(data === 'ninja') {
                this.cardInfo.bgColor = '#222222'
            }
            else if(data === 'blockchain') {
                this.cardInfo.bgColor = '#8B58F9'
            }
            else {
                this.cardInfo.bgColor = '#F33355'
            }  
        });
    }
}
</script>

<style>
.name {
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    justify-content: space-between;
    position: absolute;
    left: 16px;
    top: 173px;
    width: 345px;
}
.name div {
    text-align: left;
}
.name div+div {
    text-align: right;
}
.name div p {
    font-weight:400;
    font-size: 1.125em;
    line-height: 0.2em;
}
.name div p:first-child {
    font-size: 0.75em;
}
.wifi {
    position: absolute;
    left: 19px;
    top: 23px;
    transform: matrix(-1, 0, 0, 1, 0, 0);
}
.vendor {
    position: absolute;
    left: 329px;
    top: 28px;
}
.card-number {
    position: absolute;
    left: 16px;
    top: 105px;
    font-size: 1.9em;
    line-height: 0;
}
.container {
    width: 23.875rem;
    height: 15.063rem;
    box-shadow: 0px 4px 8px rgb(153 153 153);
    border-radius: 0.5rem;
    position: relative;
}

</style>