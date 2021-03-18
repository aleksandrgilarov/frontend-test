<template>
  <div id="app">
    <div class="container-fluid full-height flex-center flex-column bg-grey">
      <div class="row">
        <div class="col">
          <h1 class="mb-5 brand-primary-color">Technical task</h1>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div class="group m-md-2">
            <input id="name" type="text" placeholder="Enter your name" class="w-100">
            <label for="name">Enter your name</label>
          </div>
          <b-dropdown
              text="Select test"
              block
              class="m-md-2"
              menu-class="w-100"
          >
            <b-dropdown-item href="#">Action</b-dropdown-item>
            <b-dropdown-item href="#">Another action</b-dropdown-item>
            <b-dropdown-item href="#">Something else here</b-dropdown-item>
          </b-dropdown>
          <div class="m-2">
            <button class="btn btn-block rounded-pill bg-brand-primary">Start</button>
          </div>
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
  .group {
    position: relative;
  }
  input {
    display: block;
    transition: all 0.1s;
    font-size: $text-size;
    padding: 5px 10px;
  }
  input:placeholder-shown + label {
    opacity:0;
    transform: translateY(100%);
  }
  input + label {
    position: absolute;
    top: -18px;
    left: 10px;
    transition: all 0.1s;
    opacity:1;
    font-size: $text-size-sm;
  }
  input:focus {
    outline:0;
    border-color:$primary;
  }
}
</style>
