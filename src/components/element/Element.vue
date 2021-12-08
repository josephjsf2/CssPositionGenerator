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
      sendDataTimeout: null,
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
        document.addEventListener('keydown', this.handleArrowKeys);
        document.addEventListener('mousedown', this.handlemouseDown);

      } else {
        // stop controlling this element
        this.removeAllEventListener();
      }
    }
  },
  unmounted() {
    this.removeAllEventListener();
  },
  methods: {
    toggle() {
      this.toggleSelect(this.element.id)
    },
    removeAllEventListener() {
      document.removeEventListener('keydown', this.handleArrowKeys);
    },
    handlemouseDown(event) {
      if (event.button === 0) {
        this.toggleSelect(this.element.id);
        document.removeEventListener('mousedown', this.handlemouseDown);
      }
    },
    handleArrowKeys(event) {
      console.log(event)
      event.preventDefault();
      if (event.altKey){
        const rotate = event.shiftKey? 10: 1;
        switch(event.key) {
          case 'ArrowRight': {
            this.rotation.deg += rotate;
            break;
          }
          case 'ArrowLeft': {
            this.rotation.deg -= rotate;
            break;
          }
        }
      } else {
        const move = event.shiftKey? 10: 1;
        switch(event.key) {
          case 'ArrowDown': {
            this.position.top += move;
            break;
          }
          case 'ArrowUp': {
            this.position.top -= move;
            break;
          }
          case 'ArrowRight': {
            this.position.left += move;
            break;
          }
          case 'ArrowLeft': {
            this.position.left -= move;
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