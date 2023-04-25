<template>
  <div class="question_container">
    <div class="question_title">
        <h2>write your first question</h2>
        <input type="text">
    </div>
    <div class="answer_types">
        <h2>choose the type of your answer</h2>
        <div class="answerType" v-for="answer in answerTypes" :key="answer.type">
            <input type="radio" :value="answer.type">
            <label for="">{{ answer.label }}</label>
        </div>
    </div>

  </div>
</template>

<script>
export default {
    data() {
        return {
            answerTypes: [
                {type: 'radio', label: 'Radio Button'},
                {type: 'checkbox', label: 'Checkbox'},
                {type: 'text', label: 'Text Area'}
            ]
        }
    }

}
</script>

<style scoped>
.question_container {
   padding: 50px 0;
}
h2 {
    font-size: 30px;
    color: #2C3333;
}
.question_title input {
    margin-left: 20px;
    height: 40px;
    width: 500px;
    font-size: 20px;
    font-weight: 500;
    color: #2C3333;
    border: 2px solid #A5C9CA;
    border-radius: 8px;
}
.answerType {
    margin-left: 5px;
    display: flex;
    align-items: center;
    margin-bottom: 15px;
}
.answerType input {
    width: 50px;
    height: 25px;
}
label {
    font-size: 20px;
    font-weight: 500;
    color: #2C3333;
}
</style>