<template>
    <b-jumbotron header="Question:">
        <template>
            <p v-html="currentQuestion.question"></p>
        </template>

        <p v-show="answered" :class="[ resultInfo.result ? 'correct' : 'incorrect' ]">
            {{ resultInfo.message }}
        </p>
        <hr class="my-4">

        <b-list-group>
            <b-list-group-item
                    v-for="(answer, index) in answers"
                    :key="index"
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                    v-html="answer"
            >
            </b-list-group-item>
        </b-list-group>

        <b-button
                variant="success"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
        >
            Submit
        </b-button>
        <b-button variant="primary" @click="next">Next question</b-button>
    </b-jumbotron>
</template>

<script>
    import _ from 'lodash';

    export default {
        name: "QuestionBox",
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function,
        },

        data() {
            return {
                resultInfo: {},
                selectedIndex: null,
                correctIndex: null,
                answered: false,
                shuffledAnswers: [],
            }
        },

        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers];
                answers.push(this.currentQuestion.correct_answer);

                return answers;
            }
        },

        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null;
                    this.answered = false;
                    this.shuffleAnswers();
                }
            }
        },

        methods: {
            selectAnswer(index) {
                if (!this.answered) {
                    this.selectedIndex = index;
                }
            },
            submitAnswer() {
                let isCorrect = false;

                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true;
                }
                this.answered = true;
                this.setResultInfo(isCorrect);
                this.increment(isCorrect);
            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
                this.shuffledAnswers = _.shuffle(answers);
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
            },
            answerClass(index) {
                let answerClass = '';

                if (!this.answered && this.selectedIndex === index) {
                    answerClass = 'selected';
                } else if (this.answered && this.correctIndex === index) {
                    answerClass = 'correct';
                } else if (this.answered &&
                    this.selectedIndex === index &&
                    this.correctIndex !== index
                ) {
                    answerClass = 'incorrect';
                }

                return answerClass;
            },
            setResultInfo(isCorrect) {
                if (isCorrect) {
                    this.resultInfo.result = true;
                    this.resultInfo.message = 'Good job! You\'re right.';
                } else {
                    this.resultInfo.result = false;
                    this.resultInfo.message = 'Whoops! Good luck next time.';
                }
            }
        },
    }
</script>

<style scoped>
    .btn {
        margin: 0 5px;
    }

    .list-group {
        margin-bottom: 15px;
    }

    .list-group-item:not(.selected):not(.correct):not(.incorrect):hover {
        cursor: pointer;
        background: #EEE;
    }

    .selected {
        background: #2c3e50;
        color: #EEEEEE;
    }

    .correct {
        background: lightgreen;
    }

    .incorrect {
        background: lightcoral;
    }
</style>