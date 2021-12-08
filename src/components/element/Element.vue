<template>
  <div @click="toggle()" class="elem" :class="{'selected': element.select}" :style="computedStyles">
    <span class="class-name" :title="element.class">{{ element.class }}</span>
  </div>
</template>

<script>
export default {
  props: {
    element: Object,
    select: Boolean
  },
  data() {
    return {
      styles: {},
      position: {left: 30, top: 30},
      rotation: {deg: 0},
      rotate: 1, //rotate step
      move: 1,  // move step
      sendDataTimeout: null,
      alt: false, //is pressing alt
    }
  },
  computed: {
    computedStyles() {
      const styles = {
        top: this.position.top + 'px',
        left: this.position.left + 'px',
        width: this.element.size.w + 'px',
        height: this.element.size.h + 'px',
        borderRadius: this.element.borderRadius + 'px',
        backgroundColor: this.element.bgColor,
        transform: `rotate(${this.rotation.deg}deg)`
      }

      return styles;
    }
  },
  watch: {
    select(val) {
      if (val) {
        // start controlling this element
        document.addEventListener('keydown', this.handleShiftDown);
        document.addEventListener('keyup', this.handleShiftUp);
        document.addEventListener('keydown', this.handleAltDown);
        document.addEventListener('keyup', this.handleAltUp);
        document.addEventListener('keydown', this.handleArrowKeys);

      } else {
        // stop controlling this element
        document.removeEventListener('keydown', this.handleShiftDown);
        document.removeEventListener('keydown', this.handleShiftUp);
        document.addEventListener('keydown', this.handleAltDown);
        document.addEventListener('keyup', this.handleAltUp);
        document.removeEventListener('keydown', this.handleArrowKeys);
      }
    }
  },
  methods: {
    toggle() {
      this.toggleSelect(this.element.id)
    },
    startControl() {

    },
    handleShiftDown(event) {
      if (event.key === 'Shift') {
        this.move = 10;
        this.rotate = 10;
      }
    },
    handleShiftUp(event) {
      if (event.key === 'Shift') {
        this.move = 1;
        this.rotate = 1;
      }
    },
    handleAltDown(event) {
      if (event.key === 'Alt') {
        this.alt = true;
      }
    },
    handleAltUp(event) {
      if (event.key === 'Alt') {
        this.alt = false;
      }
    },
    handleArrowKeys(event) {
      event.preventDefault();
      if (this.alt){
        switch(event.key) {
          case 'ArrowRight': {
            this.rotation.deg += this.rotate;
            break;
          }
          case 'ArrowLeft': {
            this.rotation.deg -= this.rotate;
            break;
          }
        }
      } else {
        switch(event.key) {
          case 'ArrowDown': {
            this.position.top += this.move;
            break;
          }
          case 'ArrowUp': {
            this.position.top -= this.move;
            break;
          }
          case 'ArrowRight': {
            this.position.left += this.move;
            break;
          }
          case 'ArrowLeft': {
            this.position.left -= this.move;
            break;
          }
        }
      }
      
      if (this.sendDataTimeout) {
        clearTimeout(this.sendDataTimeout);
      }
      this.sendDataTimeout = setTimeout(() => {
        this.setPosition(this.position, this.element.id);
        this.setRotation(this.rotation, this.element.id)
      }, 3000)
    }
  },
  inject: [ 'toggleSelect', 'setPosition', 'setRotation' ]

}
</script>

<style scoped>

  .elem {
    position: absolute;
    cursor: pointer;
  }
  .elem.selected .class-name {
    color: #5EC8A1;
  }
  .class-name {
    display: block;
    position: absolute;
    bottom: 0;
    left: 100%;
    width: 100px;
    font-size: 0.5rem;
    font-weight: bold;
    flex-shrink: 0;
    color: #3c3c3c;
  }
</style>