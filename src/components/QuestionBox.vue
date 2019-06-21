<template>
    <div class="question-answer-container">
        <b-container>
            <b-jumbotron>
                <template slot="lead">
                    {{currentQuestion.question}}
                </template>

                <hr class="my-4">
                <b-list-group>
                    <b-list-group-item
                            :class="answerClass(index)"
                            v-for="(answer , index) in shuffledOptions" :key="index" @click="selectAnswer(index)">
                        {{answer}}
                    </b-list-group-item>
                </b-list-group>

                <b-button variant="primary" @click="submitAnswer"
                :disabled="selectedIndex===null || answered"
                >Submit</b-button>
                <b-button @click="next" variant="success" href="#">Next</b-button>
            </b-jumbotron>
        </b-container>
    </div>
</template>

<script>
    import _ from 'lodash'

    export default {
        name: "QuestionBox",
        data() {
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledOptions: [],
                numTotal:0,
                answered:false
            }
        },
        props: [
            'currentQuestion',
            'next',
            'increment'
        ],
        watch: {
            /*currentQuestion(){
                this.selectedIndex = null
                this.shuffleOptions()
            }*/
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null
                    this.answered = false
                    this.shuffleOptions()
                }
            }
        },
        methods: {
            selectAnswer(index) {
                this.selectedIndex = index
            },
            shuffleOptions() {
                this.shuffledOptions = _.shuffle(this.options)
                this.correctIndex=this.shuffledOptions.indexOf(this.currentQuestion.correct_answer)
                return this.shuffledOptions;
            },
            submitAnswer() {
                let isCorrect = false
                if(this.selectedIndex===this.correctIndex){
                    isCorrect = true
                }
                this.answered = true
                 this.increment(isCorrect)
            },
            answerClass(index){
                let answerClass = ''
                if(!this.answered && this.selectedIndex === index){
                    answerClass = 'selected'
                }
                else if(this.answered && this.correctIndex === index){
                    answerClass = 'correct'
                }
                else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
                    answerClass = 'incorrect'
                }
                return answerClass
            }
        },
        computed: {
            options() {
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        }
    }
</script>

<style lang="scss" scoped>
    .list-group {
        margin-bottom: 15px;

        .list-group-item {
            &:hover {
                background-color: #eee;
                cursor: pointer;
            }
        }
    }

    .btn {
        margin: 0 5px;
    }

    .selected {
        background-color: lightblue;
    }

    .correct {
        background-color: lightgreen;
    }

    .incorrect {
        background-color: lightcoral;
    }
</style>