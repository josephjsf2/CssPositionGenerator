<template>
  <li class="elem-item"><button :class="{'selected': element.select}" @click="toggle()">{{ element.class }}</button></li>
</template>

<script>
export default {
  props: [ 'element' ],
  data() {
    return {
      isSelected: false
    }
  },
  methods: {
    toggle() {
      this.toggleSelect(this.element.id)
    }
  },
  inject: [ 'toggleSelect' ]
}
</script>

<style scoped>
  .elem-item {
    width: 100%;
    padding: 0.5rem;
    background: #fff;
    border-radius: 5px;
  }
  .elem-item button {
    background: transparent;
    padding: 0.5rem;
    border: 2px solid #3c3c3c;
  }
  .elem-item button:hover {
    color: #ccc;
    border-color: #ccc;
  }
  .elem-item button.selected {
    color: #5EC8A1;
    border-color: #5EC8A1;
  }
</style>