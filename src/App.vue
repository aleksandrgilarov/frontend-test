<template>
  <div
    id="app"
    class="container-fluid full-height flex-center flex-column bg-grey"
  >
    <Title :title="title" />
    <FirstStep v-if="!showQuestions" @start-test="startTest" :tests="availableTests"/>
    <div v-if="showQuestions">
      <Question :question="question" @next-question="nextQuestion()"/>
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
    startTest(name) {
      this.name = name;
      this.questions = this.getTestQuestions();
      this.question = this.questions[this.step];
      this.showQuestions = true;
      this.title = this.questions[this.step].title;
    },
    getTestQuestions() {
      return [
        {
          id: 1715,
          title:
            "Who is The Elven Sword-Wielding, Green Clothed Hero in the Legend of Zelda series?",
        },
        {
          id: 2243,
          title:
            "In what game series are Golden Rings used as life-energy and money",
        },
        {
          id: 3193,
          title: 'What part of an Xbox 360 causes the "Red Ring of Death"?',
        },
        {
          id: 6001,
          title: "NES was short for....?",
        },
        {
          id: 8696,
          title:
            "How many forms does The Final Boss of The Legend of Dragoon Have?",
        },
        {
          id: 14998,
          title: "What was the first home console?",
        },
      ];
    },
    getTests() {
      //Implement get request
    },
    nextQuestion() {
      this.step++;
      this.question = this.questions[this.step];
      this.title = this.questions[this.step].title;
      console.log(this.step)
    }
  },
  data() {
    return {
      title: "Technical task Printful",
      showQuestions: false,
      name: "",
      availableTests: [],
      questions: [],
      question: {},
      step: 0,
    };
  },
  created() {
    this.availableTests = [
      {
        id: 1,
        title: "Video Games",
      },
      {
        id: 2,
        title: "Numbers",
      },
      {
        id: 3,
        title: "Movies",
      },
    ];
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
