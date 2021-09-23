<template>
    <section class="cardform">
        <form action="" @submit.prevent="save">
            <label for="cardnumber">card number</label>
            <input type="text" id="cardnumber" v-model.lazy="changeNumber" 
            placeholder="xxxx xxxx xxxx xxxx"
            v-model="cardInfo.cardNumber" v-mask="'#### #### #### ####'" required>
            <p v-show="numberError">card number must be 16 digits</p>
            <br><br>
            <label for="cardholder-name">cardholder name</label>
            <input type="text" id="cardholder-name" v-model.lazy="changeName" 
            placeholder="FIRSTNAME LASTNAME" v-on:keypress="isLetter($event)" 
            v-model="cardInfo.cardHolderName" required>
            <p v-show="nameError">name must be between 4 and 25 characters</p>
            <br><br>
            <div>
                <div>
                    <label for="valid">valid thru</label>
                    <input type="text" v-mask="'##/##'" placeholder="MM/YY" id="valid" 
                    v-model.lazy="changeValid" v-model="cardInfo.valid" required>
                    <p v-show="validError">{{ validErrorText }}</p>
                </div>
                <div>
                    <label for="ccv">ccv</label>
                    <input type="text" v-mask="'###'" placeholder="xxx" id="ccv" 
                    v-model.lazy="changeCcv" v-model="cardInfo.ccv" required>
                    <p v-show="ccvError">ccv must be 3 digits</p>
                </div>
            </div>
            <br>
            <label for="vendor">vendor</label>
            <select id="vendor" v-model="cardInfo.vendor" required v-model.lazy="changeVendor">
                <option disabled value="" selected>Please select one</option>
                <option value="bitcoin">bitcoin inc</option>
                <option value="ninja">ninja bank</option>
                <option value="blockchain">block chain inc</option>
                <option value="evil">evil corp</option>
            </select>
            <button type="submit">add card</button>
        </form>
    </section>
</template>

<script>
import { bus } from '../main';

export default {
    name: 'CardForm',
    data() {
        return {
            cardInfo: {
                cardNumber: '',
                cardHolderName: '',
                valid: '',
                ccv: '',
                vendor: '',
                bgColor: '',
                textColor: '',
                wifi: '',
                icon: ''
            },
            cards: [],
            changeNumber: '',
            changeName: '',
            changeValid: '',
            changeVendor: '',
            changeCcv: '',
            numberError: false,
            nameError: false,
            validError: false,
            ccvError: false,
            validErrorText: ''
        }
    },
    watch: {
        changeNumber: function (value) {
            if(value.length != 19) this.numberError = true
            else this.numberError = false
            bus.$emit('changeCardNumber', value);
        },
        changeName: function (value) {
            if(value.length > 25 || value.length < 5) this.nameError = true
            else this.nameError = false
            bus.$emit('changeCardName', value);
        },
        changeValid: function (value) {
            const year = String(new Date().getFullYear());
    
            if(parseInt(value.slice(0,2)) > 12) {
                this.validError = true;
                this.validErrorText = 'month must be 01-12';
            }
            else if(value.length !== 5) {
                this.validError = true;
                this.validErrorText = 'must be 4 digits';
            }
            else if(parseInt(year.slice(2,4)) > parseInt(value.slice(3,5))) {
                this.validError = true;
                this.validErrorText = 'year must be valid';
            }
            else {
                this.validError = false
            }
            bus.$emit('changeCardValid', value);
        },
        changeCcv: function (value) {
            if(value.length !== 3) {
                this.ccvError = true
            }
            else {
                this.ccvError = false
            }
        },
        changeVendor: function (value) {
            bus.$emit('changeCardVendor', value);
        }
    },
    methods: {
        isLetter(e) {
            let char = String.fromCharCode(e.keyCode);
            if(/^[A-Öa-ö ]+$/.test(char)) return true; 
            else e.preventDefault();
        },
        save: function () {
            if(!this.numberError && !this.nameError && !this.ccvError && !this.validError) {
                this.cardInfo.textColor = '#FFFFFF';
                this.cardInfo.wifi = require('../assets/chip-light.svg')
                
                if(this.cardInfo.vendor === 'ninja') {
                    this.cardInfo.bgColor = '#222222',
                    this.cardInfo.icon = require('../assets/vendor-ninja.svg')
                }
                else if (this.cardInfo.vendor === 'evil') {
                    this.cardInfo.bgColor = '#F33355',
                    this.cardInfo.icon = require('../assets/vendor-evil.svg')
                }
                else if (this.cardInfo.vendor === 'blockchain') {
                    this.cardInfo.bgColor = '#8B58F9',
                    this.cardInfo.icon = require('../assets/vendor-blockchain.svg')
                }
                else {
                    this.cardInfo.bgColor = '#ffae34',
                    this.cardInfo.textColor = '#000000',
                    this.cardInfo.wifi = require('../assets/chip-dark.svg'),
                    this.cardInfo.icon = require('../assets/vendor-bitcoin.svg')
                }
                if (window.localStorage.getItem('cardinfo')) {
                    try {
                        this.cards = JSON.parse(localStorage.getItem('cardinfo'));
                    } catch(e) {
                        window.localStorage.removeItem('cardinfo');
                    }
                }
                const copyCard = Object.assign({}, this.cardInfo);
                this.cards.push(copyCard);
                window.localStorage.setItem('cardinfo', JSON.stringify(this.cards));
                this.cardInfo.cardNumber = '';
                this.cardInfo.cardHolderName = '';
                this.cardInfo.valid = '';
                this.cardInfo.ccv = '';
            }
        }
    }
}
</script>

<style>
.cardform {
    width: 382px;
    margin-top: 3rem;
}
.cardform p {
    font-size: 0.6em;
    line-height: 0;
    position: absolute;
    color: red;
}
.cardform div div p {
    margin-top: 5.5rem;
}
label {
    font-size: 0.8em;
    line-height: 1.5em;
}
input, select {
    min-height: 56px;
    width: fill-available;
    border-radius: 8px;
    border: 1px solid black;
    font-family: 'PT Mono';
    font-weight: bold;
    font-size: 1.125em;
    padding: 0 1em;
    text-transform: uppercase;
}
input::placeholder {
    color: black;
}
.cardform div {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
.cardform div div {
    width: 45%;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
}
button {
    background: black;
    color: white;
    width: 100%;
    height: 72px;
    margin-top: 2.5rem;
    border-radius: 0.5rem;
    border: none;
    font-size: 1.5em;
    font-family: 'PT Mono';
    font-weight: bold;
    cursor: pointer;
    text-transform: uppercase;
}
</style>