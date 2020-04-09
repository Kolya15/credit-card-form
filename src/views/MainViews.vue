<template>
    <div class="main-wrapper">
        <div class="wrapper-card">
            <Card ref="card" :card-data="cardSettings" :focus-element-style="focusElementStyle"></Card>
        </div>
        <div class="wrapper-form">
            <CardForm @set-settings="setSettings($event)" @focus-input="focusInputStyle($event)"></CardForm>
        </div>
    </div>
</template>

<script>
    import Card from "../components/Card";
    import CardForm from "../components/CardForm";

    export default {
        name: "MainViews",
        data() {
            return {
                cardSettings: {},
                focusElementStyle: {},
            }
        },
        components: {
            Card,
            CardForm
        },
        methods:{
            setSettings(event){
                this.cardSettings = {...event};
            },
            focusInputStyle (event){
                this.focusElementStyle = event;
            }
        }
    }
</script>

<style scoped>
    .main-wrapper {
        max-width: 600px;
        margin: 50px auto 0 auto;
        width: 100%;
    }
    .wrapper-card{
        max-width: 450px;
        height: 270px;
        width: 100%;
        margin: 0 auto -130px auto;
    }
    .wrapper-form{
        background: #fff;
        box-shadow: 0 30px 60px 0 rgba(90, 116, 148, 0.4);
        border-radius: 10px;
        height: auto;
        padding: 180px 35px 35px 35px;
    }
    @media (max-width: 600px) {
        .main-wrapper {
            max-width: 90%;
        }
        .wrapper-card{
            max-width: 70%;
            height: 240px;
            display: flex;
            justify-content: center;
        }
    }
    @media (max-width: 380px) {
        .wrapper-card{
            max-width: 80%;
        }
    }
</style>