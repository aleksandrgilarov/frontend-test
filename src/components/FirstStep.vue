<template>
  <div class="row">
    <div class="col">
      <b-alert v-model="showAlert" variant="danger" dismissible class="w-100 mb-4">
        Name or test not selected!
      </b-alert>
      <form @submit.prevent="startTest">
        <div class="group mb-2">
          <input
            id="name"
            type="text"
            v-model="name"
            placeholder="Enter your name"
            class="w-100"
          />
          <label for="name">Enter your name</label>
        </div>
        <b-form-select
          v-model="selectedTest"
          :options="options"
          class="mb-2"
        ></b-form-select>
        <input
          type="submit"
          class="btn btn-block rounded-pill bg-brand-primary custom-button"
          value="Start test"
        />
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "FirstStep",
  methods: {
    startTest() {
      if (!this.name || !this.selectedTest) {
        this.showAlert = true;
      } else {
        this.$emit("start-test", this.name, this.selectedTest);
      }
    },
    async getTests() {
      const res = await fetch(
        "https://printful.com/test-quiz.php?action=quizzes"
      );

      return await res.json();
    },
  },
  data() {
    return {
      name: "",
      tests: [],
      selectedTest: null,
      options: [
        { value: null, text: "Please select a Test" },
      ],
      showAlert: false,
    };
  },
  async created() {
    this.tests = await this.getTests();
    this.tests.forEach((test) => {
      this.options.push({ value: test.id, text: test.title });
    });
  },
};
</script>

<style scoped></style>
