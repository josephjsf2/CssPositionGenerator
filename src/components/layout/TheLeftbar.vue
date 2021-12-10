<template>
  <div class="container" :class="{'closed': isBarOpen && !isBarPinned, 'pinned': isBarPinned}">
    <button v-if="!isBarPinned" class="close-btn" @click="toggleBar()"><span></span></button>
    <button class="pin-btn" :class="{'pinned': isBarPinned}" @click="pinBar()">
      <svg xmlns="http://www.w3.org/2000/svg" width="20" height="25" viewBox="0 0 25 28.5">
      <g id="グループ_13" data-name="グループ 13" transform="translate(-417 -99)">
        <g id="多角形_1" data-name="多角形 1" transform="translate(441 110) rotate(180)" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path d="M10.809.661a1,1,0,0,1,1.382,0L21.2,9.277A1,1,0,0,1,20.508,11H2.492A1,1,0,0,1,1.8,9.277Z" stroke="none"/>
          <path d="M 11.49999523162842 2.767620086669922 L 4.984323501586914 9 L 18.01567459106445 9 L 11.49999523162842 2.767620086669922 M 11.5 0.3838119506835938 C 11.74895477294922 0.3838119506835938 11.99790954589844 0.4762639999389648 12.1912202835083 0.6611690521240234 L 21.19906044006348 9.277359008789062 C 21.85062980651855 9.900599479675293 21.40949058532715 10.99999904632568 20.5078296661377 10.99999904632568 L 2.492170333862305 10.99999904632568 C 1.590509414672852 10.99999904632568 1.149370193481445 9.900599479675293 1.800939559936523 9.277359008789062 L 10.8087797164917 0.6611690521240234 C 11.00209045410156 0.4762639999389648 11.25104522705078 0.3838119506835938 11.5 0.3838119506835938 Z" stroke="none" fill="#637e77"/>
        </g>
        <g id="多角形_2" data-name="多角形 2" transform="translate(417 108)" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path d="M11.839.581a1,1,0,0,1,1.321,0l9.85,8.668A1,1,0,0,1,22.35,11H2.65a1,1,0,0,1-.661-1.751Z" stroke="none"/>
          <path d="M 12.5000057220459 2.66413402557373 L 5.300155639648438 8.999995231628418 L 19.69983863830566 8.999995231628418 L 12.5000057220459 2.66413402557373 M 12.5000057220459 0.332066535949707 C 12.73589324951172 0.332066535949707 12.97177505493164 0.4151601791381836 13.16063022613525 0.581355094909668 L 23.01054954528809 9.249284744262695 C 23.70269012451172 9.858365058898926 23.27190017700195 10.99999523162842 22.34992980957031 10.99999523162842 L 2.650070190429688 10.99999523162842 C 1.728099822998047 10.99999523162842 1.297309875488281 9.858365058898926 1.989450454711914 9.249284744262695 L 11.83936977386475 0.581355094909668 C 12.02822494506836 0.4151649475097656 12.26411819458008 0.332066535949707 12.5000057220459 0.332066535949707 Z" stroke="none" fill="#637e77"/>
        </g>
        <line id="線_11" data-name="線 11" y2="8" transform="translate(429.5 118.5)" fill="none" stroke="#637e77" stroke-linecap="round" stroke-width="2"/>
      </g>
    </svg>
    </button>
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
      isBarPinned: false
    }
  },
  methods: {
    toggleBar() {
      this.isBarOpen = !this.isBarOpen;
    }, 
    pinBar() {
      this.isBarPinned = !this.isBarPinned;
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
    flex-shrink: 0;
  }
  .container.pinned {
    position: relative;
    box-shadow: 3px 0 6px rgba(0,0,0,0.1);
  }
  .container.closed {
    left: -300px;
  }
  .container button {
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
    padding: 0;
    outline: none;
  }
  .container button:hover {
    opacity: 1;
    box-shadow: 0 3px 6px rgba(0,0,0,0.3);
  }
  .closed .close-btn {
    position: fixed;
    border-radius: 20px;
    left: 15px;
    top: 15px;
    transition: 1s;
  }

  .close-btn {
    position: fixed;
    left: 310px;
    top: 15px;
    transition: 1s;
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
    transition: 0.5s
  }
  .closed .close-btn span::before {
    transform: rotate(0deg);
    top: -6px;
  }
  .closed .close-btn span::after {
    transform: rotate(0deg);
    top: 6px;
  }
  .pin-btn {
    position: fixed;
    left: 310px;
    top: 60px;
    transition: 1s;
  }
  .closed .pin-btn {
    left: 15px;
  }
  .pin-btn.pinned {
    transform: rotate(180deg);
    top: 15px;
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
