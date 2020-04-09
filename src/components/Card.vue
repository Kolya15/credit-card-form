<template>
    <div class="wrapper-card">
        <div class="card-item__side front" ref="front">
            <div class="card-item__focus" :style="focusElementStyle"></div>
            <div class="card-item__cover" >
                <img :src="imagesCard" class="card-item__bg">
            </div>
            <div class="card-item__wrapper">
                <div class="card-item__top">
                    <img src="../assets/images/chip.png">
                    <div class="card-item__type">
                        <transition name="slide-fade-up">
                            <img :src="getCardType" :key="getCardType">
                        </transition>
                    </div>
                </div>
                <div class="wrapper-number-card">
                    <div class="box-number-card" ref="cardNumber">
                        <span v-for="(item, $index) in defaultNumberCard" :key="$index">
                            <transition name="slide-fade-up">
                            <div
                                    class="card-item__numberItem"
                                    v-if="$index > 3 && $index < 12 && cardSettings.cardNumber.length > $index && item.trim() !== ' '"
                            >*</div>
                            <div class="card-item__numberItem"
                                 :key="$index"
                                 v-else-if="cardSettings.cardNumber.length > $index">
                                 {{cardSettings.cardNumber[$index]}}
                            </div>
                            <div
                                    class="card-item__numberItem"
                                    v-else
                                    :key="$index + 1"
                            >{{item}}</div>
                                </transition>
                        </span>
                    </div>
                </div>
                <div class="card-item__content">
                    <div class="card-holder" ref="cardHolder">
                        <label>Card Holder</label>
                        <transition name="slide-fade-up">
                            <div v-if="cardSettings.cardHolders.length" key="1" class="wrapper-cars-holder__text">
                                <transition-group name="slide-fade-right">
                            <span v-for="(item, $index) in cardSettings.cardHolders" :key="$index"
                                  class="letter-card-holders">
                                {{item}}
                            </span>
                                </transition-group>
                            </div>
                            <div v-else key="2">Full Name</div>
                        </transition>
                    </div>
                    <div class="expires" ref="cardExpires">
                        <label>Expires</label>
                        <div>
                            <transition name="slide-fade-up">
                                <span v-if="cardSettings.cardMonth" :key="cardSettings.cardMonth">{{cardSettings.cardMonth}}</span>
                                <span v-else key="2">MM</span>
                            </transition>
                            /
                            <transition name="slide-fade-up">
                                <span v-if="cardSettings.cardYear" :key="cardSettings.cardYear">{{String(cardSettings.cardYear).slice(2,4)}}</span>
                                <span v-else key="2">YY</span>
                            </transition>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="card-item__side back" ref="back">
            <div class="card-item__cover">
                <img :src="imagesCard" alt="" class="card-item__bg">
            </div>
            <div class="card-item__band"></div>
            <div class="card-item__cvv">
                <div class="card-item__cvvTitle">cvv</div>
                <div class="card-item__cvvBand"><span v-for="(item, $index) in cardSettings.cardCvv"
                                                      :key="$index">*</span></div>
                <div class="card-item__type">
                    <img :src="getCardType">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Card",
        data() {
            return {
                currentCardBackground: Math.floor(Math.random()* 25 + 1),
                cardSettings: {
                    cardNumber: '',
                    cardHolders: '',
                    cardMonth: null,
                    cardYear: null,
                    cardCvv: null,
                },
                defaultNumberCard: '################',
            }
        },
        props: {
            cardData: Object,
            focusElementStyle: Object,
        },
        computed: {
            imagesCard () {
                return require(`@/assets/images/${this.currentCardBackground}.jpeg`)
            },
            getCardType () {
                let number = this.cardSettings.cardNumber;
                let nameBank = "visa"; // default type;
                let re = new RegExp("^4");
                if (number.match(re) != null) nameBank = "visa";

                re = new RegExp("^(34|37)");
                if (number.match(re) != null) nameBank = "amex";

                re = new RegExp("^5[1-5]");
                if (number.match(re) != null) nameBank = "mastercard";

                re = new RegExp("^6011");
                if (number.match(re) != null) nameBank = "discover";

                re = new RegExp('^9792')
                if (number.match(re) != null) nameBank = 'troy'

                return require(`@/assets/images/${nameBank}.png`)
            },
        },
        watch: {
            cardData(newVal) {
                this.cardSettings = {...newVal}
                return this.cardSettings;
            }
        }
    }
</script>

<style scoped>
    *{
        box-sizing: border-box;
    }
    .card-item__side {
        position: absolute;
        margin: inherit;
        border-radius: 15px;
        overflow: hidden;
        box-shadow: 0 20px 60px 0 rgba(14, 42, 90, 0.55);
        transform: perspective(2000px) rotateY(0deg) rotateX(0deg) rotate(0deg);
        transform-style: preserve-3d;
        transition: all 0.8s cubic-bezier(0.71, 0.03, 0.56, 0.85);
        backface-visibility: hidden;
        height: inherit;
        padding: 5px;
        max-width: inherit;
        width: inherit;
    }

    .back {
        transform: perspective(2000px) rotateY(-180deg) rotateX(0deg) rotate(0deg);
        z-index: 2;
    }

    .active-front {
        transform: perspective(1000px) rotateY(180deg) rotateX(0deg) rotateZ(0deg);
    }

    .active-back {
        transform: perspective(1000px) rotateY(0) rotateX(0deg) rotateZ(0deg);
    }
    .card-item__focus {
        position: absolute;
        z-index: 3;
        border-radius: 5px;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        transition: all 0.35s cubic-bezier(0.71, 0.03, 0.56, 0.85);
        opacity: 0;
        pointer-events: none;
        overflow: hidden;
        padding: 10px;
        background-color: rgba(0,0,0,.5);
        border: 2px solid rgba(255, 255, 255, 0.65);
    }
    .card-item__cover {
        position: absolute;
        height: 100%;
        background-color: #1c1d27;
        left: 0;
        top: 0;
        width: 100%;
        border-radius: 15px;
        overflow: hidden;
    }

    .card-item__bg {
        max-width: 100%;
        display: block;
        max-height: 100%;
        height: 100%;
        width: 100%;
        object-fit: cover;
        background-color: #000000;
        opacity: 0.7;
    }

    .card-item__wrapper {
        position: relative;
        z-index: 4;
        height: 100%;
        text-shadow: 7px 6px 10px rgba(14, 42, 90, 0.8);
        user-select: none;
    }

    .card-item__wrapper img,
    .card-item__type img {
        width: 70px;
        height: auto;
    }

    .card-item__top {
        max-height: 86px;
        padding: 15px;
        display: flex;
        justify-content: space-between;
    }

    .wrapper-number-card {
        display: flex;
        justify-content: center;
        margin-top: 30px;
    }
    .box-number-card{
        padding-bottom: 10px;
    }
    .box-number-card span {
        font-size: 25px;
        color: #ffffff;
        font-weight: bold;
    }

    .box-number-card span:nth-child(4n) {
        margin-right: 35px;
    }

    .card-item__numberItem {
        display: inline-block;
    }

    .card-item__content {
        display: flex;
        justify-content: space-between;
        margin-top: 30px;
        padding: 0 10px;
    }

    .card-holder {
        width: 80%;
        padding-bottom: 10px;
    }
    .wrapper-cars-holder__text {
        line-height: 1;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        text-transform: uppercase;
    }
    .letter-card-holders {
        display: inline-block;
        text-transform: uppercase;
        min-width: 8px;
    }

    .expires {
        width: 20%;
        padding-bottom: 10px;
    }

    .card-holder label,
    .expires label {
        font-size: 14px;
        color: #ffffff;
    }

    .card-holder div,
    .expires div {
        font-size: 20px;
        color: #ffffff;
    }
    .expires span{
        min-width: 25px;
        display: inline-block;
        /*width: 30px;*/
    }

    .card-item__band {
        background-color: #000000;
        width: 100%;
        height: 50px;
        margin-top: 30px;
        position: relative;
        z-index: 2;
    }

    .card-item__cvv {
        text-align: right;
        position: relative;
        z-index: 2;
        padding: 10px 15px 15px 15px;
    }

    .card-item__cvvTitle {
        color: #ffffff;
        font-size: 18px;
        padding-right: 10px;
    }

    .card-item__cvvBand {
        background-color: #ffffff;
        border-radius: 5px;
        width: 100%;
        height: 50px;
        position: relative;
        font-size: 18px;
        padding: 10px 10px 0 0;
        z-index: 2;
    }

    .back .card-item__type {
        margin-top: 30px;
    }

    .slide-fade-up-enter-active {
        transition: all 0.30s ease-in-out;
        transition-delay: 0.1s;
        position: relative;
    }

    .slide-fade-up-leave-active {
        transition: all 0.30s ease-in-out;
        position: absolute;
    }

    .slide-fade-up-enter {
        opacity: 0;
        transform: translateY(15px);
        pointer-events: none;
    }

    .slide-fade-up-leave-to {
        opacity: 0;
        transform: translateY(-15px);
        pointer-events: none;
    }

    .slide-fade-right-enter-active {
        transition: all 0.30s ease-in-out;
        transition-delay: 0.1s;
        position: relative;
    }

    .slide-fade-right-leave-active {
        transition: all 0.25s ease-in-out;
        position: absolute;
    }

    .slide-fade-right-enter {
        opacity: 0;
        transform: translateX(10px) rotate(45deg);
        pointer-events: none;
    }

    .slide-fade-right-leave-to {
        opacity: 0;
        transform: translateX(-10px) rotate(45deg);
        pointer-events: none;
    }
    @media (max-width: 600px) {
        .card-item__top {
            max-height: 75px;
        }
        .card-item__wrapper img{
            width: 60px;
        }
        .wrapper-number-card{
            margin-top: 10px;
        }
        .box-number-card span[data-v-54cdc180] {
            font-size: 22px;
        }
        .box-number-card span:nth-child(4n) {
            margin-right: 20px;
        }
        .card-item__content {
            margin-top: 25px;
        }
        .card-item__content label{
            font-size: 13px;
        }
        .card-item__content div {
            font-size: 17px;
        }
        .card-item__content .card-holder {
            width: 70%;
        }
        .card-item__content .expires {
            width: 30%;
        }
        .back .card-item__band[data-v-54cdc180] {
            height: 40px;
        }
        .back .card-item__cvvBand{
            height: 40px;
        }
        .back .card-item__type img{
            width: 60px;
        }
    }
    @media (max-width: 450px){
        .box-number-card span:nth-child(4n) {
            margin-right: 10px;
        }
        .card-item__content label{
            font-size: 11px;
        }
        .card-item__content .card-holder {
            width: 65%;
        }
        .card-item__content .expires {
            width: 35%;
        }
    }
    @media (max-width: 380px) {
        .box-number-card span {
            font-size: 18px !important;
        }
    }
</style>