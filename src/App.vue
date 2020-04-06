<template>
    <div id="app">
        <MainVuews></MainVuews>
    </div>
</template>

<script>
  import MainVuews from "./views/MainVuews";

    export default {
        name: 'App',
        components: {
            MainVuews,
        }
    }
</script>

<style>
    @import '~materialize-css/dist/css/materialize.min.css';
</style>
