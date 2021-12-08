<template>
  <base-card :grow="true">
    <template #content>
      <ul class="container">
        <element-item v-for="elem in elements" :key="elem.id" :element="elem"></element-item>
      </ul>
    </template>    
  </base-card>
</template>

<script>
import ElementItem from './ElementItem.vue';

export default {
  components: {
    ElementItem
  },
  inject: [ 'elements' ]
}
</script>

<style scoped>
  .container {
    width: 100%;
    height: 300px;
    overflow-y: scroll;
  }
</style>