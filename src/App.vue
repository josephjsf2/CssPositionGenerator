<template>
  <the-leftbar></the-leftbar>
  <the-main :bg-url="bgUrl" :size="bgSize"></the-main>

  <base-alert :danger="alertDanger" v-if="hasAlert"><p>{{ alertText }}</p></base-alert>
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
      alertTimeout: null,
      alertDanger: false
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
      let num = 0;
      while(this.elements.find(el => el.class === elem.class)) {
        num = num + 1;
        elem.class = num === 1 ? elem.class + `(${num})`: elem.class.replace(/(?<=[(])\d(?=[)])/, num);
      }

      this.elements.push(elem);
    },
    removeElement(id) {
      const element = this.elements.find(elem => elem.id === id);
      const index = this.elements.indexOf(element);
      this.elements.splice(index, 1);
    },
    toggleSelect(id) {
      // if (this.selectedElem && this.selectedElem.id !== id) {
      //   this.selectedElem.select = false;
      // }
      const elem = this.elements.find(elem => elem.id === id);
      elem.select = !elem.select;
      // if (elem.select) {
      //   this.selectedElem = elem;
      // }
    },
    deselectAll() {
      this.elements.forEach(elem => elem.select = false);
    },
    setPosition(position, id) {
      this.elements.find(elem => elem.id === id).position = position;
    },
    setRotation(rotation, id) {
      this.elements.find(elem => elem.id === id).rotation = rotation;
    },
    showAlert(text, isDanger) {
      this.alertDanger = isDanger? true: false;

      this.alertText = text;
      this.hasAlert = true;
      if (this.alertTimeout) {
        clearTimeout(this.alertTimeout);
      }
      this.alertTimeout = setTimeout(() => {
        this.hasAlert = false;
      }, 1000)
    },
    updateElement(oldId, data) {
      const element = this.elements.find(elem => elem.id === oldId);
      element.size = data.size;
      element.class = data.class;
      element.bgColor = data.bgColor;
      element.borderRadius = data.borderRadius;
    }
  },
  provide() {
    return {
      uploadUrl: this.uploadUrl,
      setBgSize: this.setBgSize,
      elements: this.elements,
      addElement: this.addElement,
      removeElement: this.removeElement,
      toggleSelect: this.toggleSelect,
      deselectAll: this.deselectAll,
      setPosition: this.setPosition,
      setRotation: this.setRotation,
      showAlert: this.showAlert,
      updateElement: this.updateElement
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
  display: flex;
}
button, input, label {
  font-family: 'Roboto', sans-serif, Arial;
  font-weight: bold;
}
</style>
