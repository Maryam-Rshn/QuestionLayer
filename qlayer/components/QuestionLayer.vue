<template>
  <div class="question_container">
    <div v-if="!isFinished" class="form_container">
        <div class="question_title">
            <h2>write the title of question {{ question.id }}</h2>
            <input type="text" v-model="question.title">
        </div>
        <div class="answer_types">
            <h2>choose the type of the answer</h2>
            <div class="answerType" v-for="answer in answerTypes" :key="answer.id">
                <input name="answer_type" type="radio" :value="answer.type" :id="answer.id" v-model="question.answer_type">
                <label :for="answer.id">{{ answer.label }}</label>
            </div>
        </div>
        <div v-if="isMultipleChoices">
            <h2>write choices for the answer</h2>
            <div class="answers">
                <input type="text" v-model="answer">
                <button @click="addAnswerChoices()" :disabled="!answer">{{ isEditting ? 'Save' : 'Add' }}</button>
                <div class="answerChoices_container">
                    <div class="answerChoices" v-for="(answer, index) in question.answers" :key="index">
                        <div>
                            <input v-if="question.answer_type === 'radio' " name="answer" type="radio" :value="answer">
                            <input v-if="question.answer_type === 'checkbox' " name="answer" type="checkbox" :value="answer">
                            <label>{{ answer }}</label>
                        </div>
                        <div class="action_buttons">
                            <button @click="askToDeleteAnswer(index)">Delete</button>
                            <button @click="askToEditAnswer(index)">Edit</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="previous_answer" v-if="isAnswerRadio">
            <h2>Choose an answer from previous question</h2>
            <div>
                <select v-for="(question, index) in answerScenarios" :key="index" name="selectedAnswer" v-model="selectedAnswer" @change="submitAnswer(index)">
                    <option disabled value="">title of question: {{ question.title }}</option>
                    <option v-for="answer in question.answers" :key="answer" :value="answer">{{ answer }}</option>
                </select>
            </div>
        </div>
        <div class="previous_answer" v-if="notRadioScenarios.length > 0">
            <h2>Choose a question from previous layer</h2>
            <div>
                <select name="selectedAnswer" v-model="selectedAnswer" >
                    <option v-for="answer in notRadioScenarios" :key="answer" :value="answer">{{ answer }}</option>
                </select>
                <button @click="submitNotRadioAnswer()">Submit</button>
            </div>
        </div>
        <div class="save_finish_buttons">
            <button class="save_question" @click="saveQuestion" :disabled="!isSavingAllowed">Save Question</button>
            <button class="askToFinish" @click="isFinished = !isFinished" :disabled="questionLayers.length < 1">Finish</button>
        </div>
    </div>
    <div v-if="isFinished" class="finish">
        <h1>Your questions are successfully added !</h1>
    </div>
    layer {{ layer }} <br><br>
    question layer{{ questionLayers }} <br><br>
    answer scenario{{ answerScenarios }} 
  </div>
</template>

<script>

export default {
    data() {
        return {
            answer: '',
            isSavingActive: false,
            isEditting: false,
            index: null,
            isFinished: false,
            isAnswerRadio: false,
            selectedAnswer: '',
            selectedRadioAnswer: {question: '', answer: ''},
            answerScenarios: [],
            notRadioScenarios: [],
            
            answerTypes: [
                {id: 1, type: 'radio', label: 'Radio Button'},
                {id: 2, type: 'checkbox', label: 'Checkbox'},
                {id: 3, type: 'text', label: 'Text Area'}
            ],
            question: {
                id: 1,
                title: '',
                answer_type: '',
                answers: [],
                askedBase: '',
                askedBaseRadio: []
            },
            layer:[],
            questionLayers: [],
        }
    },
    computed: {
        isMultipleChoices() {
            if(this.question.answer_type === "text" || !this.question.answer_type) {
                return false
            } else {
                return true
            }
        },
        isSavingAllowed() {
            if(this.question.title && this.question.answer_type) {
                if( this.question.answer_type !== "text") {
                    if(this.question.answers.length < 2) {
                        return false
                    } else {
                        return true
                    }
                }
                else {
                    return true
                } 
            } 
            else {
                return false
            }
        },
        handleNotRadioScenarios() {
            if(this.questionLayers.length > 1) {
                const index = this.questionLayers.length - 1
                const test = []
                this.questionLayers[index].filter((layer) => {
                    if(layer.answer_type !== 'radio') {
                        test.push(layer.title)
                    }
                })
                return test
            }
        },
    },
    methods: {
        addAnswerChoices() {
            if(this.isEditting) {
                this.question.answers[this.index] = this.answer
                this.answer = ''
                this.isEditting = false
            } else {
                if(this.answer) {
                    this.question.answers.push(this.answer)
                    this.answer = ''
                }
            }
        },
        askToEmptyQuestion() {
            this.question.id += 1
            this.question.title = ''
            this.question.answer_type = ''
            this.question.answers = []
        },
        saveQuestion() {
            if(this.questionLayers.length < 1) {
                if(this.question.answer_type === 'radio') {
                    this.isAnswerRadio = true
                    this.answerScenarios = [{title: this.question.title, answers: [...this.question.answers]}]
                }
                this.layer.push({...this.question})
                this.questionLayers.push(this.layer)
                this.layer = []
                this.askToEmptyQuestion()
            }
            else {
                if(this.isAnswerRadio) {
                    this.layer.push({...this.question})
                    this.askToEmptyQuestion()
                    if(this.answerScenarios.length < 1) {
                        this.questionLayers.push(this.layer)
                        this.layer= []
                        this.isAnswerRadio = false
                        if(this.handleNotRadioScenarios.length > 0) {
                            this.notRadioScenarios = [...this.handleNotRadioScenarios]
                        }
                        this.questionLayers[this.questionLayers.length - 1].map((layer)=> {
                            if(layer.answer_type === 'radio') {
                                this.isAnswerRadio = true
                                this.answerScenarios.push({title: layer.title, answers: [...layer.answers]})
                            }
                        })
                    }
                }
                else {
                    this.layer.push({...this.question})
                    this.askToEmptyQuestion()
                    if(this.notRadioScenarios.length < 1) {
                        this.questionLayers.push(this.layer)
                        this.layer= []
                        this.isAnswerRadio = false
                        this.questionLayers[this.questionLayers.length - 1].filter((layer)=> {
                            if(layer.answer_type === 'radio') {
                                this.isAnswerRadio = true
                                this.answerScenarios.push(layer)
                            }
                        })
                    }
                }
            }
        },
        askToEditAnswer(index) {
            this.isEditting = true
            this.index = index
            console.log(index, 'this is ndex')
            this.answer = this.question.answers[index]
        },
        askToDeleteAnswer(index) {
            this.question.answers.splice(index,1)
        },
        submitAnswer(index) {
            this.question.askedBase = this.selectedAnswer
            this.answerScenarios[index].answers.splice(this.answerScenarios.indexOf(this.selectedAnswer), 1)
            if(this.answerScenarios[index].answers.length === 0) {
                this.answerScenarios.splice(index, 1)
            }
            this.selectedAnswer = ''
        },
        submitNotRadioAnswer() {
            this.question.askedBase = this.selectedAnswer
            this.notRadioScenarios.splice(this.notRadioScenarios.indexOf(this.selectedAnswer), 1)
            this.selectedAnswer = ''
        }
    },
    mounted() {
        console.log(this.selectedType, 'this is selected type');
        console.log(this.question.answers, 'this is answers');
    }

}
</script>

<style lang="scss" scoped>
.question_container {
   margin: 40px -110px;
   padding: 20px 110px;
   background-color: #E7F6F2;
   display: flex;
   width: 100%;
   justify-content: space-between;
}
h2 {
    font-size: 30px;
    color: #2C3333;
}
.question_title  {
    input {
        margin-left: 20px;
        height: 40px;
        width: 500px;
        font-size: 20px;
        font-weight: 500;
        color: #2C3333;
        border: 2px solid #A5C9CA;
        border-radius: 8px;
    }
}
.answer_types {
    button {
        height: 40px;
        border: 2px solid #A5C9CA;
        background-color: #A5C9CA;
        border-radius: 8px;
        font-size: 15px;
        font-weight: 700;
        width: 70px;
        color: #E7F6F2;
        margin-left: 20px;
        cursor: pointer;
        &:disabled {
            background-color: #E7F6F2;
            color: #A5C9CA;
        }
    }
}
.answerType {
    margin-left: 5px;
    display: flex;
    align-items: center;
    margin-bottom: 15px;
    label {
        font-size: 20px;
        font-weight: 500;
        color: #2C3333;
    }
    input {
        width: 50px;
        height: 25px;
        &:checked {
            background-color: pink;
        }
    }
    
}
.answers {
    margin: 10px 0 0 20px;
    input {
        height: 35px;
        width: 442px;
        border: 2px solid #A5C9CA;
        border-radius: 8px;
        margin-right: 8px;
        font-size: 17px;
        color: #2C3333;
    }
    button {
        height: 40px;
        border: 2px solid #A5C9CA;
        background-color: #A5C9CA;
        border-radius: 8px;
        font-size: 15px;
        font-weight: 700;
        width: 50px;
        color: #E7F6F2;
        cursor: pointer;
        &:disabled {
            background-color: #E7F6F2;
            color: #A5C9CA;
        }
    }
}
.save_question {
    height: 40px;
    margin-top: 80px;
    border: 2px solid #A5C9CA;
    background-color: #A5C9CA;
    border-radius: 8px;
    font-size: 20px;
    font-weight: 700;
    padding: 0 20px;
    color: #E7F6F2;
    cursor: pointer;
    &:disabled {
        background-color: #E7F6F2;
        color: #A5C9CA;
    }
}
.askToFinish {
    height: 40px;
    margin-top: 80px;
    border: 2px solid #A5C9CA;
    background-color: #A5C9CA;
    border-radius: 8px;
    font-size: 20px;
    font-weight: 700;
    padding: 0 20px;
    color: #E7F6F2;
    cursor: pointer;
    &:disabled {
        background-color: #E7F6F2;
        color: #A5C9CA;
    }
}
.save_finish_buttons{
    display: flex;
    gap: 15px;
}
.answerChoices_container {
    margin-top: 30px;
}
.answerChoices{
    display: flex;
    justify-content: space-between;
    align-items: center;
    div{
        display: flex;
        align-items: center;
        margin-top: 15px;
    }
    input {
        height: 25px;
        width: 50px;
    }
    label {
        font-size: 20px;
    }
    button {
        height: 30px;
        width: 60px;
        background-color: #395B64;
        border-color: #395B64;
        color: #A5C9CA;
    }
}
.action_buttons {
    display: flex;
    gap: 8px;
}

.finish {
    h1{
        font-size: 25px;
        color: #395B64;

    }
}
.previous_answer{
    div {
        display: flex;
        justify-content: space-between;
    }
    select {
        width: 400px;
        height: 40px;
        font-size: 18px;
        padding: 0 15px;
        margin-left: 20px;
        border: 2px solid #A5C9CA;
        border-radius: 8px;
        color: #2C3333;
    }
    button {
        height: 40px;
        border: 2px solid #A5C9CA;
        background-color: #A5C9CA;
        border-radius: 8px;
        font-size: 15px;
        font-weight: 700;
        color: #E7F6F2;
        cursor: pointer;
        &:disabled {
            background-color: #E7F6F2;
            color: #A5C9CA;
        }
    }
}

</style>