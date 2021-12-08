<template>
  <div class="container">
    <base-card>
      <template #content>  
        <div v-if="!!bgUrl" class="bg-img">
          <img :width="size.w" :height="size.h" class="img" :src="bgUrl">
          <Element v-for="elem in elements" :key="elem.id" :element="elem" :select="elem.select" />
        </div>
      </template>
    
    </base-card>
  </div>
</template>

<script>
import Element from '../element/Element.vue';

export default {
  components: {
    Element
  },
  props: [ 'bg-url', 'size' ],
  data() {
    return {
      
    }
  },
  inject: [ 'elements' ]

}

</script>
<style scoped>
  .container {
    padding: 1rem;
    height: 100vh;
  }
  /* card is global class */
  .container .card {
    height: 100%;
  }

  .bg-img {
    width: 100%;
    height: 100%;
    overflow: scroll;
    position: relative;
    box-shadow: 0 0 5px rgba(0,0,0,0.5);
  }
</style>