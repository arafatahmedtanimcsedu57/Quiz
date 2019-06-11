<template>
    <div>
        <b-jumbotron>
            <template slot="lead">
                {{ currentQuestion.question}}
            </template>

            <hr class="my-4">

            <div class="answers-list">
                <b-list-group>
                    <b-list-group-item 
                        v-for="(answer, index) in shuffledAnswers" 
                        :key="index"
                        @click.prevent="selectAnswer(index)" 
                        :class="[selectedIndex === index ? 'selected' : '']"
                    >
                        {{ answer }}
                    </b-list-group-item>
                </b-list-group>
            </div>

            <b-button 
                variant="primary"
                @click="submitAnswer"

            >
                Submit
            </b-button>
            <b-button variant="success" href="#" @click="next">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    props:{
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data(){
        return{
            selectedIndex: null,
            correctIndex:null,
            shuffledAnswers: []
        }
    },

    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },

    watch:{
        currentQuestion:{
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.shuffleAnswers()
            }
        }
    },

    methods:{
        selectAnswer(index){
            this.selectedIndex = index
        },

        submitAnswer(){
            let isCorrect = false

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }

            this.increment( isCorrect )
        },

        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        }
    },
    mounted(){
        this.shuffleAnswers()
    }    
}
</script>

<style scoped>
    .list-group{
        margin-bottom: 15px;
    }

    .list-group-item:hover{
        background: rgba(151, 151, 151, 0.1);
        cursor: pointer;
    }

    .btn {
        margin: 3px;
    }

    .selected{
        background-color: lightblue
    }

    .correct{
        background-color: lightgreen
    }

    .incorrect{
        background-color: lightred
    }
</style>
