<template>
    <div id="app">
        <Header
                :numCorrect="numCorrect"
                :numTotal="numTotal"
        />

        <b-container class="bv-example-row mt-5">
            <b-row>
                <b-col sm="6" offset="3">
                    <Loader v-if="loading"></Loader>
                    <QuestionBox
                            v-else-if="questions.length"
                            :currentQuestion="questions[index]"
                            :next="next"
                            :increment="increment"
                    />
                    <p v-else>Something went wrong. Please, check your internet connection.</p>
                </b-col>
            </b-row>
        </b-container>

    </div>
</template>

<script>
    import Header from "./components/Header";
    import QuestionBox from "./components/QuestionBox";
    import Loader from "./components/Loader";
    import axios from "axios";

    export default {
        name: 'app',
        components: {
            Header,
            QuestionBox,
            Loader
        },

        data() {
            return {
                questions: [],
                index: 0,
                numCorrect: 0,
                numTotal: 0,
                loading: true
            }
        },

        methods: {
            next() {
                this.index++;
            },
            increment(isCorrect) {
                if (isCorrect) {
                    this.numCorrect++;
                }
                this.numTotal++;
            }
        },

        mounted() {
            axios.get('https://opentdb.com/api.php?amount=10&category=15&type=multiple')
                .then(response => {
                    this.questions = response.data.results;
                    this.loading = false;
                })
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
