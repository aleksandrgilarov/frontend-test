<template>
  <p :key="option.id" :v-for="option in options">{{ option.title }}</p>
</template>

<script>
export default {
  name: "Options",
  props: {
    options: {
      type: Array,
    }
  }
}
</script>

<style scoped>

</style>
