<template>
  <dialog class="dialog" open>
    <slot></slot>
  </dialog>
</template>

<style scoped>
  .dialog {
    display: block;
    width: 50%;
    min-height: 50px;
    background: rgb(94 200 161);
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