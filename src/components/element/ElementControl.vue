<template>
  <base-card>
    <template #title>
      <h2>Element</h2>
    </template>
    <template #content>
      <div class="container">
        <form class="element-form" @submit.prevent="add()">
          <div class="input-grp">
            <label class="txt-label" for="cssClass">css class:</label>
            <input class="txt-input" type="text" v-model="cssClass" id="cssClass">
          </div>
          <div class="input-grp half">
            <label class="txt-label" for="width">W:</label>
            <input class="txt-input" type="text" v-model="width" id="width">
          </div>
          <div class="input-grp half">
            <label class="txt-label" for="height">H:</label>
            <input class="txt-input" type="text" v-model="height" id="height">
          </div>
          <div class="input-grp">
            <label class="txt-label" for="bgColor">bg color:</label>
            <input class="txt-input" type="text" v-model="bgColor" id="bgColor">
          </div>
          <div class="input-grp">
            <label class="txt-label" for="borderRadius">border radius:</label>
            <input class="num-input" type="number" min="0" max="20" step="1" v-model="borderRadius" id="borderRadius" placeholder="0~20">
          </div>
          <div class="btn-wrap">
            <base-button :primary="true" :solid="true" >Add</base-button>
          </div>
        </form>
      </div> 
    </template>
  </base-card>
</template>

<script>
export default {
  data() {
    return {
      cssClass: '',
      width: null,
      height: null,
      bgColor: 'blue',
      borderRadius: null
    }
  },
  methods: {
    alert() {
      window.alert('hi')
    },
    add() {
      if (!this.cssClass) {
        this.showAlert('A css class must be given to the new element!', 'danger');
        return;
      }

      if(!this.width || !this.height) {
        this.showAlert('A size must be given to the new element!', 'danger');
        return;
      }

      if (!this.bgColor) {
        this.bgColor = 'blue';
      }

      const newElem = {
        class: this.cssClass,
        size: { w: this.width, h: this.height },
        bgColor: this.bgColor,
        borderRadius: this.borderRadius,
        id: Math.floor(Math.random() * 1000000),
        position: {top: 0, left: 0},
        rotation: {deg: 0},
        select: false
      }
      console.log(newElem)
      this.addElement(newElem);
    }
  },
  inject: [ 'addElement', 'showAlert' ]  // element array
}
</script>

<style scoped>
  .container {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }
  .element-form {
    display: flex;
    flex-wrap: wrap;
  }
  .btn-wrap {
    margin-top: 0.5rem;
  }
</style>