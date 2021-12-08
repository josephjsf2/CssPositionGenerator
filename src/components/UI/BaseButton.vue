<template>
  <button class="btn" :class="{'primary': primary, 'solid': solid}">
    <slot></slot>
  </button>
</template>
<script>
export default {
  props: {
    primary: {
      type: Boolean,
      default: false
    },
    solid: {
      type: Boolean,
      default: false
    },
  }
}

</script>
<style scoped>
  .btn {
    height: 2rem;
    border-style: none;
    border-radius: 1rem;
    border: 2px solid #707070;
    background: transparent;
    color: #707070;
    padding: 0.1rem 0.5rem;
    font-size: 1rem;
    cursor: pointer;
  }
  .btn:hover {
    border-color: #928f8f;
    color: #928f8f;
  }
  .btn.solid {
    background: #707070;
    color: #fff;
  }
  .btn.solid:hover {
    background: #928f8f;
  }
  .btn.primary {
    border-color: #5EC8A1;
    color: #5EC8A1;
  }
  .btn.primary:hover {
    border-color: #8eddc0;
    color: #8eddc0;
  }
  .btn.solid.primary {
    background: #5EC8A1;
    color: #fff;
  }
  .btn.solid.primary:hover {
    background: #8eddc0;
  }

</style>