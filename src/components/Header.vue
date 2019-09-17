<template>
    <div>
        <b-nav tabs align="center">
            <b-nav-item disabled><b>Sparkqy - Vue.js + BootstrapVue Quiz App</b></b-nav-item>
            <b-nav-item disabled>Counter: <b>{{ numCorrect }}/{{ numTotal }}</b></b-nav-item>
        </b-nav>
    </div>
</template>

<script>
    export default {
        name: "Header",
        props: [
            'numCorrect',
            'numTotal',
        ]
    }
</script>

<style scoped>

</style>