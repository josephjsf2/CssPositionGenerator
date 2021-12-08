<template>
  <the-leftbar></the-leftbar>
  <the-main :bg-url="bgUrl" :size="bgSize"></the-main>

  <base-alert v-if="hasAlert"><p>{{ alertText }}</p></base-alert>
</template>

<script>
import TheLeftbar from './components/layout/TheLeftbar.vue';
import TheMain from './components/layout/TheMain.vue';
export default {
  name: 'App',
  data() {
    return {
      bgUrl: '',
      bgSize: {w: '0', h: '0'},
      elements: [],
      selectedElem: {},
      alertText: '',
      hasAlert: false,
      alertTimeout: null
    }
  },
  components: {
    TheLeftbar,
    TheMain
  },
  methods: {
    uploadUrl(url) {
      this.bgUrl = url;
    },
    setBgSize(w, h) {
      this.bgSize = {w, h};
    },
    addElement(elem) {
      this.elements.push(elem);
    },
    removeElement(id) {
      this.elements = this.elements.filter(elem => elem.id === id);
    },
    toggleSelect(id) {
      if (this.selectedElem && this.selectedElem.id !== id) {
        this.selectedElem.select = false;
      }
      const elem = this.elements.find(elem => elem.id === id);
      elem.select = !elem.select;
      if (elem.select) {
        this.selectedElem = elem;
      }
    },
    setPosition(position, id) {
      this.elements.find(elem => elem.id === id).position = position;
      console.log(this.elements)
    },
    setRotation(rotation, id) {
      this.elements.find(elem => elem.id === id).rotation = rotation;
    },
    showAlert(text) {
      this.alertText = text;
      this.hasAlert = true;
      if (this.alertTimeout) {
        clearTimeout(this.alertTimeout);
      }
      this.alertTimeout = setTimeout(() => {
        this.hasAlert = false;
      }, 1000)
    }
  },
  provide() {
    return {
      uploadUrl: this.uploadUrl,
      setBgSize: this.setBgSize,
      elements: this.elements,
      addElement: this.addElement,
      toggleSelect: this.toggleSelect,
      setPosition: this.setPosition,
      setRotation: this.setRotation,
      showAlert: this.showAlert
    }
  }
}
</script>

<style>

#app {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Roboto', sans-serif, Arial;
}
button, input, label {
  font-family: 'Roboto', sans-serif, Arial;
  font-weight: bold;
}
</style>
