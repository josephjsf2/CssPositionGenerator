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
    <div v-if="hasDescrp" class="descrp">
      <button class="close-btn" @click="close()">
        <svg xmlns="http://www.w3.org/2000/svg" width="11.414" height="11.414" viewBox="0 0 11.414 11.414">
          <g id="グループ_51" data-name="グループ 51" transform="translate(-1380.793 -1031.793)">
            <line id="線_17" data-name="線 17" x2="10" y2="10" transform="translate(1381.5 1032.5)" fill="none" stroke="#707070" stroke-linecap="round" stroke-width="1"/>
            <line id="線_18" data-name="線 18" x1="10" y2="10" transform="translate(1381.5 1032.5)" fill="none" stroke="#707070" stroke-linecap="round" stroke-width="1"/>
          </g>
        </svg>
      </button>
      <TheDescription />
    </div>
  </div>
</template>

<script>
import Element from '../element/Element.vue';
import TheDescription from '../layout/TheDescription.vue';

export default {
  components: {
    Element,
    TheDescription
  },
  props: [ 'bg-url', 'size' ],
  data() {
    return {
      hasDescrp: true
    }
  },
  methods: {
    close() {
      this.hasDescrp = false;
    }
  },
  inject: [ 'elements' ]

}

</script>
<style scoped>
  .container {
    padding: 1rem;
    height: 100vh;
    width: 100%;
  }
  /* card is global class */
  .container .card {
    height: 100%;
  }

  .bg-img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: scroll;
    box-shadow: 0 0 5px rgba(0,0,0,0.5);
  }

  .descrp {
    position: fixed;
    bottom: 10px;
    right: 10px;
    opacity: 0.9;
  }
  .descrp .close-btn {
    position: absolute;
    right: 0;
    top: 0;
    border: none;
    width: 24px;
    height: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background: #eee;
  }
  .descrp .close-btn:hover {
    box-shadow: 0 3px 6px rgba(0,0,0,0.3);
  }
</style>