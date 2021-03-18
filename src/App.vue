<template>
  <div id="app">
    <div class="container-fluid full-height centered bg-grey">
      <div class="row">
        <div class="col">
          <h1 class="m-md-2 brand-primary-color">Technical task</h1>
          <input type="text" placeholder="Enter your name" class="m-md-2 w-100">
          <b-dropdown
              text="Select test"
              block
              class="m-2"
              menu-class="w-100"
          >
            <b-dropdown-item href="#">Action</b-dropdown-item>
            <b-dropdown-item href="#">Another action</b-dropdown-item>
            <b-dropdown-item href="#">Something else here</b-dropdown-item>
          </b-dropdown>
          <button class="btn btn-block rounded-pill m-md-2 bg-brand-primary">Start</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "App",
  components: {
  },
};
</script>

<style lang="scss">
@import "assets/scss/main";
#app {
  button {
    color: $white;
    border-color: transparent;
    font-size: $text-size;
    &:hover {
      opacity: .8;
      border-color: transparent;
    }
  }
  input {
    display: block;
  }
}
</style>
