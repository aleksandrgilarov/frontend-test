<template>
  <div class="row">
    <div
      class="col-md-6 col-12 options-wrapper"
      :key="option.id"
      v-for="option in options"
    >
      <input
        type="checkbox"
        :id="option.id"
        :value="option.id"
        v-model="checkedOptions"
      />
      <label :for="option.id" class="rounded-pill text-center">{{
        option.title
      }}</label>
    </div>
    <div class="col-12">
      <input
        type="button"
        @click="nextStep()"
        :disabled="checkedOptions.length === 0"
        :value="buttonLabel"
        class="btn btn-block rounded-pill font-weight-bold mt-4 bg-brand-primary custom-button"
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "Question",
  props: ["options", "buttonLabel"],
  methods: {
    async nextStep() {
      this.$emit("next-question", this.checkedOptions);
      this.checkedOptions = [];
    },
  },
  data() {
    return {
      checkedOptions: [],
    };
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/main";
.options-wrapper {
  input[type="checkbox"] {
    display: none;
  }
  label {
    background-color: lighten($primary, 10%);
    color: $white;
    padding: 10px 15px;
    border-radius: 0.5rem;
    width: 100%;
    &:hover {
      cursor: pointer;
      opacity: 0.8;
    }
  }
  input[type="checkbox"]:checked + label {
    background-color: $brand-orange;
  }
}
</style>
