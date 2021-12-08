<template>
  <dialog class="dialog" :class="{'danger': danger}" open>
    <slot></slot>
  </dialog>
</template>

<script>
export default {
  props: {
    danger: {type: Boolean, default: false}
  }
}
</script>

<style scoped>
  .dialog {
    display: block;
    width: 50%;
    min-height: 50px;
    background: rgba(94, 200, 161, 0.829);
    color: #fff;
    border: none;
    padding: 1rem;
    border-radius: 12px;
    box-shadow: 0 3px 6px rgba(0,0,0,0.3);
    position: fixed;
    left: 50vw;
    top: 10vh;
    transform: translate(-50%);
    z-index: 2;
    animation: fadeout 2s ease-out;
    animation-iteration-count: 1;
  }
  .dialog.danger {
    background: rgba(202, 30, 64, 0.829);
  }

  @keyframes fadeout {
    0% {
      top: 10vh;
    }
    30% {
      opacity: 1;
    }
    100% {
      top: 0;
      opacity: 0;
    }
  }
</style>