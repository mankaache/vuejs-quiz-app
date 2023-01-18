<template>
  <main class="app">
    <h1>The quiz</h1>
    <section class="quiz" v-if="!quizeCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.questions }}</span>
        <span class="score">score {{ score }} / {{ questions.length }}</span>
      </div>
      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="setAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>
      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? "Finish"
            : getCurrentQuestion.selected == null
            ? "Select an option"
            : "Next Question"
        }}
      </button>
    </section>
    <section v-else>
      <h2>You have finished the quiz!</h2>
      <p>Your score is {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
</template>

<script>
import { computed, ref } from "vue";
export default {
  setup() {
    const questions = ref([
      {
        questions: "what is Vue JS",
        answer: 0,
        options: ["A front end framework", "A librabry", "An ice cream Maker"],
        selected: null,
      },
      {
        questions: "what is Vuex",
        answer: 2,
        options: ["Vue with an x", "A cheese selection", "State Management"],
        selected: null,
      },
      {
        questions: "what is Vue JS",
        answer: 0,
        options: [
          "Walking in space",
          "A routing library for vue js",
          "Burger sauce",
          "quizzes",
        ],
        selected: null,
      },
    ]);
    console.log(questions);

    const quizeCompleted = ref(false);
    const currentQuestion = ref(0);

    const score = computed(() => {
      let count = 0;
      questions.value.map((q) => {
        if (q.selected == q.answer) {
          count++;
        }
      });
      return count;
    });
    const getCurrentQuestion = computed(() => {
      let question = questions.value[currentQuestion.value];
      question.index = currentQuestion.value;
      return question;
    });

    const setAnswer = (evt) => {
      questions.value[currentQuestion.value].selected = evt.target.value;
      evt.target.value = null;
    };
    const NextQuestion = () => {
      if (currentQuestion.value < questions.value.length - 1) {
        currentQuestion.value++;
      } else {
        quizeCompleted.value = true;
      }
    };

    return { questions, score, NextQuestion, getCurrentQuestion, setAnswer };
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background: #271c36;
  color: #fff;
}
</style>
