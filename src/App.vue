<template>
  <div
    id="app"
    class="container-fluid full-height flex-center flex-column bg-grey"
  >
    <Title :title="title" />
    <FirstStep v-if="!showQuestions && !lastStep" @start-test="startTest"/>
    <div v-if="showQuestions && !lastStep">
      <Question :buttonLabel="buttonLabel" :options="options" @next-question="nextQuestion"/>
      <b-progress :value="step" :max="questions.length" animated variant="success" class="mt-2" ></b-progress>
    </div>
    <p v-if="lastStep">You responded correctly to {{ this.correctAnswers.correct }} out of {{ this.correctAnswers.total }} questions</p>
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
      this.options = await this.getOptions();
      this.showQuestions = true;
      this.title = this.questions[this.step].title;
      this.step++;
    },

    async getTestQuestions(id) {
      const res = await fetch(
        `https://printful.com/test-quiz.php?action=questions&quizId=${id}`
      );

      return await res.json()
    },
    async getOptions() {
      const res = await fetch(
          `https://printful.com/test-quiz.php?action=answers&quizId=${this.quizID}&questionId=${this.question.id}`
      );

      return await res.json();
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
    async nextQuestion(checkedOptions) {
      if (this.step === this.questions.length) {
        this.title = `Thanks, ${this.name}`;
        this.lastStep = true;
      } else {
        this.question = this.questions[this.step];
        this.options = await this.getOptions();
        this.title = this.questions[this.step].title;
        this.checkedAnswers = [...this.checkedAnswers, ...checkedOptions];
        this.correctAnswers = await this.checkAnswers(this.checkedAnswers);
        this.step++;
        if (this.step === this.questions.length) {
          this.buttonLabel = "Finish";
        }
      }
    },
  },
  data() {
    return {
      title: "Technical task Printful",
      showQuestions: false,
      lastStep: false,
      name: "",
      quizID: null,
      questions: [],
      options: [],
      question: {},
      step: 0,
      correctAnswers: 0,
      checkedAnswers: [],
      buttonLabel: "Next",
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
