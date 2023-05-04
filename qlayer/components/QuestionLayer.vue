<template>
  <div class="question_container">
    <div class="form_container">
        <div class="question_title">
            <h2>write your first question</h2>
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
                <div class="answerChoices_container" v-if="question.answer_type === 'radio' ">
                    <div class="answerChoices" v-for="(answer, index) in question.answers" :key="index">
                        <div>
                            <input name="answer" type="radio" :value="answer">
                            <label>{{ answer }}</label>
                        </div>
                        <div class="action_buttons">
                            <button @click="askToDeleteAnswer(index)">Delete</button>
                            <button @click="askToEditAnswer(index)">Edit</button>
                        </div>
                    </div>
                </div>
                <div class="answerChoices_container" v-if="question.answer_type === 'checkbox' ">
                    <div class="answerChoices" v-for="(answer, index) in question.answers" :key="index">
                        <div>
                            <input name="answer" type="checkbox" :value="answer">
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
        <button class="save_question" @click="saveQuestion" :disabled="!question.title || !question.answer_type">Save Question</button>
    </div>
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
            
            answerTypes: [
                {id: 1, type: 'radio', label: 'Radio Button'},
                {id: 2, type: 'checkbox', label: 'Checkbox'},
                {id: 3, type: 'text', label: 'Text Area'}
            ],
            question: {
                id: 1,
                title: '',
                answer_type: '',
                answers: []
            },
            questions: [],
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
        saveQuestion() {
            this.questions.push({...this.question})
            console.log(this.questions, 'this is questions');
            this.question.id += 1
            this.question.title = ''
            this.question.answer_type = ''
            this.question.answers = []
        },
        askToEditAnswer(index) {
            this.isEditting = true
            this.index = index
            console.log(index, 'this is ndex')
            this.answer = this.question.answers[index]
        },
        askToDeleteAnswer(index) {
            this.question.answers.splice(index,1)
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
    // width: 100px;
    color: #E7F6F2;
    cursor: pointer;
    &:disabled {
        background-color: #E7F6F2;
        color: #A5C9CA;
    }
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
        // font-weight: 700;
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

// .result {
//     width: 50%;
//     height: 100%;
//     background-color: #E7F6F2;
// }

</style>