<template>
  <div @click="toggle()" class="elem" :class="{'selected': element.select}" :style="computedStyles" ref="elemRef">
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
      position: this.element.position,
      rotation: this.element.rotation,
      sendDataTimeout: null,
      tempCopy: null
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
        document.addEventListener('keydown', this.handleDeleteKeys);
        document.addEventListener('keydown', this.handleDuplicateElem)
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
      document.removeEventListener('keydown', this.handleDeleteKeys);
      document.removeEventListener('keydown', this.handleDuplicateElem)
    },
    handleDeleteKeys(event) {
      if (event.key === 'Delete') {
        event.preventDefault();
        this.removeElement(this.element.id)
      }
    },
    handleArrowKeys(event) {
      if (['ArrowRight', 'ArrowLeft', 'ArrowDown', 'ArrowUp'].includes(event.key)){
        event.preventDefault();
      }
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
    },
    handleDuplicateElem(event) {
      if (event.ctrlKey && event.key === 'c') {
        this.tempCopy = {...this.element};
      }
      if (event.ctrlKey && event.key === 'v' && this.tempCopy) {
        // give new id
        this.tempCopy.id = Math.floor(Math.random() * 1000000);

        // copy position and rotation as well
        this.tempCopy.position = {...this.position};
        this.tempCopy.rotation = {...this.rotation};
        
        this.addElement(this.tempCopy);

        // cancel current selection and select onto the new element
        this.toggleSelect(this.element.id)
        this.tempCopy.select = false;

      }
    }
  },
  inject: [ 'toggleSelect', 'setPosition', 'setRotation', 'addElement', 'removeElement' ]

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