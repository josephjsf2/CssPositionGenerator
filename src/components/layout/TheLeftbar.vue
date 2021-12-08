<template>
  <div class="container" :class="{'closed': isBarOpen}">
    <button class="close-btn" @click="toggleBar()"><span></span></button>
    <h1 class="title">CSS Position Generator</h1>
    <bg-control></bg-control>
    <element-control></element-control>
    <element-list v-if="elements && elements.length > 0"></element-list>
    <output-control></output-control>
  </div>
</template>
<script>
import BgControl from '../background/BgControl.vue';
import ElementControl from '../element/ElementControl.vue';
import ElementList from '../element/ElementList.vue';
import OutputControl from '../output/OutputControl.vue';

export default {
  components: {
    BgControl,
    ElementControl,
    ElementList,
    OutputControl
  },
  data() {
    return {
      isBarOpen: false,
    }
  },
  methods: {
    toggleBar() {
      this.isBarOpen = !this.isBarOpen;
    }
  },
  inject: [ 'elements' ]  // element array
}
</script>

<style scoped>
  .container {
    padding: 1rem;
    width: 300px;
    height: 100vh;
    background: #eeeeee;
    display: flex;
    flex-direction: column;
    align-items: center;
    position: fixed;
    gap: 0.5rem;
    top: 0;
    left: 0;
    transition: 1s;
    z-index: 2;
    overflow-y: scroll;
  }
  .container.closed {
    left: -300px;
  }
  .closed .close-btn {
    position: fixed;
    border-radius: 20px;
    left: 15px;
    top: 15px;
    transition: 1s;
  }

  .close-btn {
    padding: 0;
    position: fixed;
    left: 310px;
    top: 1rem;
    cursor: pointer;
    border: none;
    width: 40px;
    height: 40px;
    background: #eeeeee;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 20px;
    opacity: 0.5;
    transition: 1s;
    outline: none;
  }
  .close-btn:hover {
    opacity: 1;
    box-shadow: 0 3px 6px rgba(0,0,0,0.3);
  }
  .close-btn span {
    display: block;
    position: relative;
    width: 20px;
    height: 2px;
    background: transparent;
  }
  .close-btn span::before {
    content: '';
    position: absolute;
    top: 50%;
    left: 0;
    height: 2px;
    width: 100%;
    background: rgb(99, 126, 119);
    transform: rotate(45deg);
    transition: 1s;
  }
  .close-btn span::after {
    content: '';
    position: absolute;
    top: 50%;
    left: 0;
    height: 2px;
    width: 100%;
    background: rgb(99, 126, 119);
    transform: rotate(-45deg);
    transition: 1s;
  }
  .closed .close-btn span {
    background: rgb(99, 126, 119);
    transition: 1s
  }
  .closed .close-btn span::before {
    transform: rotate(0deg);
    top: -6px;
  }
    .closed .close-btn span::after {
    transform: rotate(0deg);
    top: 6px;
  }

  .title {
    font-size: 1.5rem;
    text-shadow: 0 3px 3px rgba(0,0,0,0.1);
  }
  .container .control-card {
    width: 100%;
    height: auto;
  }

  ::-webkit-scrollbar {
    display: none;
  }
</style>
