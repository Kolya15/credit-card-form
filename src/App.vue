<template>
    <div id="app">
        <MainViews></MainViews>
    </div>
</template>

<script>
  import MainViews from "./views/MainViews";

    export default {
        name: 'App',
        components: {
            MainViews,
        }
    }
</script>

<style>
    @import '~materialize-css/dist/css/materialize.min.css';
</style>
