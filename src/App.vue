<template>
  <div
    id="app"
    class="container-fluid full-height flex-center flex-column bg-grey"
  >
    <Title :title="title" />
    <FirstStep v-if="!showQuestions" @start-test="startTest"/>
    <div v-if="showQuestions">
      <Question :quizid="quizID" :question="question" @next-question="nextQuestion"/>
      <b-progress :value="step" :max="questions.length" animated variant="success" class="mt-2" ></b-progress>
    </div>
  </div>
</template>

<script>
import FirstStep from "@/components/FirstStep";
import Title from "@/components/Title";
import Question from "@/components/Question";

export default {
  name: "App",
  components: {
    FirstStep,
    Title,
    Question,
  },
  methods: {
    async startTest(name, id) {
      this.name = name;
      this.quizID = id;
      this.questions = await this.getTestQuestions(id);
      this.question = this.questions[this.step];
      this.showQuestions = true;
      this.title = this.questions[this.step].title;
    },

    async getTestQuestions(id) {
      const res = await fetch(
        `https://printful.com/test-quiz.php?action=questions&quizId=${id}`
      );

      return await res.json()
    },
    async checkAnswers(checkedAnswers) {
      let answers = "";
      checkedAnswers.forEach((answer) => {
        answers += `&answers[]=${answer}`;
      });

      const res = await fetch(
        `https://printful.com/test-quiz.php?action=submit&quizId=${this.quizID}${answers}`
      );

      return await res.json()
    },
    nextQuestion(checkedOptions) {
      this.step++;
      this.question = this.questions[this.step];
      this.title = this.questions[this.step].title;
      this.checkedAnswers = [...this.checkedAnswers, ...checkedOptions];
      console.log(checkedOptions)
      console.log(this.checkedAnswers)
      this.correctAnswers = this.checkAnswers(this.checkedAnswers);
    },
  },
  data() {
    return {
      title: "Technical task Printful",
      showQuestions: false,
      name: "",
      quizID: null,
      questions: [],
      question: {},
      step: 0,
      correctAnswers: 0,
      checkedAnswers: [],
    };
  },
};
</script>

<style lang="scss">
@import "assets/scss/main";
#app {
  .custom-button {
    color: $white;
    border-color: transparent;
    font-size: $text-size;
    min-width: 150px;
    &:hover {
      opacity: 0.8;
      border-color: transparent;
    }
  }
  .group {
    position: relative;
  }
  input[type="text"] {
    display: block;
    transition: all 0.1s;
    font-size: $text-size;
    padding: 5px 10px;
    min-width: 250px;
  }
  input[type="text"]:placeholder-shown + label {
    opacity: 0;
    transform: translateY(100%);
  }
  input[type="text"] + label {
    position: absolute;
    top: -18px;
    left: 10px;
    transition: all 0.1s;
    opacity: 1;
    font-size: $text-size-sm;
  }
  input[type="text"]:focus {
    outline: 0;
    border-color: $primary;
  }
}
</style>
