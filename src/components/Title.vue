<template>
  <div class="row">
    <div class="col">
      <h1 class="mb-5 brand-primary-color">{{ title }}</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "Title",
  props: {
    title: {
      type: String,
      default: "Technical task",
    },
  },
};
</script>

<style scoped>
h1 {
  text-align: center;
}
</style>
