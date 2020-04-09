<template>
    <div>
        <div class="input-field">
            <label for="card-number">Card Number</label>
            <input id="card-number" type="text" maxlength="16" v-model="cardSettings.cardNumber" data-card="cardNumber"
                   @focus="focusInput($event)" @blur="blurInput()" @input="setSettings($event)">
        </div>
        <div class="input-field">
            <label for="card-holders">Card Holders</label>
            <input id="card-holders" type="text" v-model="cardSettings.cardHolders" data-card="cardHolder"  @focus="focusInput($event)" @blur="blurInput()" @input="setSettings($event)">
        </div>
        <div class="wrapper-item__bottom">
            <div class="input-field">
                <select class="icons" v-model="cardSettings.cardMonth" data-card="cardExpires"  @focus="focusInput($event)" @blur="blurInput()" @change="setSettings($event)">
                    <option value="" disabled selected>Month</option>
                    <option :value="item < 10 ? '0' + item : item" v-for="item in 12" :key="item" :disabled="item < minCardMonth">
                        {{ setCardMonth(item) }}
                    </option>
                </select>
            </div>
            <div class="input-field">
                <select class="icons" v-model="cardSettings.cardYear" data-card="cardExpires"  @focus="focusInput($event)" @blur="blurInput()" @change="setSettings($event)">
                    <option value="" disabled selected>Year</option>
                    <option v-for="(item, index) in 12" :value="presentYear + index" :key="index">{{presentYear + index}}
                    </option>
                </select>
            </div>
            <div class="input-field cvv">
                <label for="card-cvv">CVV</label>
                <input id="card-cvv" type="text" autocomplete="off" maxlength="3" data-card="cardCvv"
                       v-model="cardSettings.cardCvv" @input="setSettings($event)" @focus="flipCard()"
                       @blur="flipCard()">
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "CardForm",
        data() {
            return {
                cardSettings: {
                    cardNumber: '',
                    cardHolders: '',
                    cardMonth: '',
                    cardYear: '',
                    cardCvv: ''
                },
                presentYear: new Date().getFullYear(),
                isInputFocused: false,
                focusElementStyle: null,
            }
        },
        methods: {
            setSettings(event) {
                if (event.target.dataset.card === 'cardNumber' || event.target.dataset.card === 'cardCvv') {
                    let arrayInput = event.target.value.split('');
                    let newArray = [];
                    arrayInput.forEach(item => {
                        if (Number(item) >= 0 && Number(item) <= 9 && item != ' ') {
                            newArray.push(item);
                        }
                    });
                    this.cardSettings[event.target.dataset.card] = newArray.join('');
                }
                this.$emit('set-settings', this.cardSettings);
            },
            setCardMonth(n) {
                if (this.cardSettings.cardYear === this.presentYear){
                    this.cardSettings.cardMonth = '';
                    this.$emit('set-settings', this.cardSettings);
                }
                return n < 10 ? '0' + n : n;
            },
            flipCard() {
                this.$parent.$refs.card.$refs.front.classList.toggle('active-front')
                this.$parent.$refs.card.$refs.back.classList.toggle('active-back')
            },
            focusInput (event){
                this.isInputFocused = true;
                let targetRef = event.target.dataset.card;
                let target = this.$parent.$refs.card.$refs[targetRef];
                this.focusElementStyle = {
                    width: `${target.offsetWidth}px`,
                    height: `${target.offsetHeight}px`,
                    transform: `translateX(${target.offsetLeft}px) translateY(${target.offsetTop}px)`,
                    opacity: `1`,
                }
                this.$emit('focus-input', this.focusElementStyle);
            },
            blurInput() {
                this.isInputFocused = false;
                setTimeout(() => {
                    if (!this.isInputFocused) {
                        this.focusElementStyle = null;
                        this.$emit('focus-input', this.focusElementStyle);
                    }
                }, 400);
            }
        },
        computed: {
            minCardMonth() {
                    if (this.cardSettings.cardYear === this.presentYear) return new Date().getMonth() + 1;
                    return 1;
                },
            },
    }
</script>

<style scoped>
    .wrapper-item__bottom {
        display: flex;
        position: relative;
        justify-content: space-around;
        margin-top: 20px;
    }
    .wrapper-item__bottom::before {
        content: 'Expiration Date';
        position: absolute;
        top: -15px;
        left: 0px;
        font-size: 14px;
    }

    .wrapper-item__bottom div {
        width: 30%;
    }

    select {
        display: inline-block;
    }
    @media (max-width: 600px) {
        .wrapper-item__bottom {
            flex-wrap: wrap;
        }
        .wrapper-item__bottom .input-field{
            width: 45%;
        }
        .cvv {
            width: 100% !important;
        }
    }

</style>