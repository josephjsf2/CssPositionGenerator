<template>
  <li class="elem-item">
    <button class="toggle-btn" :class="{'selected': element.select}" @click="toggle()"><input class="elem-title" v-model="data.class" @blur="saveChange()"></button>
    <div class="action-btns">
      <button @click="openModal()" class="edit-btn">Edit</button>
      <button @click="removeElement(element.id)" class="delete-btn">Delete</button>
    </div>
  </li>
  <base-modal v-on:modal-hide="hideModal()" v-if="hasModal">
    <template #title><h2>Edit</h2></template>
    <template #content>
      <div class="edit-wrap">
         <div class="input-grp">
            <label class="txt-label" for="cssClass">css class:</label>
            <input class="txt-input" type="text" v-model="data.class" id="cssClass">
          </div>
          <div class="input-grp half">
            <label class="txt-label" for="width">W:</label>
            <input class="txt-input" type="text" v-model="data.size.w" id="width">
          </div>
          <div class="input-grp half">
            <label class="txt-label" for="height">H:</label>
            <input class="txt-input" type="text" v-model="data.size.h" id="height">
          </div>
          <div class="input-grp">
            <label class="txt-label" for="bgColor">bg color:</label>
            <input class="txt-input" type="text" v-model="data.bgColor" id="bgColor">
          </div>
          <div class="input-grp">
            <label class="txt-label" for="borderRadius">border radius:</label>
            <input class="num-input" type="number" min="0" max="20" step="1" v-model="data.borderRadius" id="borderRadius" placeholder="0~20">
          </div>
      </div>
      <div class="btn-wrap">
        <base-button :primary="true" @click="saveChange()">Save</base-button>
      </div>
    </template>
  </base-modal>
</template>

<script>
import BaseModal from '../UI/BaseModal.vue'
export default {
  components: { BaseModal },
  props: [ 'element' ],
  data() {
    return {
      data: {
        ...this.element,
        size: {...this.element}
      },
      isSelected: false,
      hasModal: false
    }
  },
  created() {

  },
  methods: {
    toggle() {
      this.toggleSelect(this.element.id)
    },
    openModal() {
      this.hasModal = true;
    },
    hideModal() {
      this.hasModal = false;
    },
    saveChange() {
      if (!this.data.size.w || !this.data.size.h) {
        alert('A size must be givent to the element!')
      }

      if (!this.data.class.trim()) {
        alert('A class name must be givent to the element!')
      }

      this.updateElement(this.element.id, this.data);
      this.hideModal();
    }
  },
  inject: [ 'toggleSelect', 'updateElement', 'removeElement' ]
}
</script>

<style scoped>
  .elem-item {
    width: 100%;
    padding: 0.5rem;
    background: #fff;
    border-radius: 5px;
    display: flex;
  }
  .elem-item .elem-title {
    border: none;
    outline: none;
    max-width: 120px;
  }
  .elem-item .elem-item .elem-title:hover {
    border-bottom: 2px solid darkslategray;
  }
  .elem-item .toggle-btn.selected .elem-title {
    color: #5EC8A1;
  }
  .elem-item button {
    cursor: pointer;
    background: transparent;
    flex-shrink: 0;
  }
  .elem-item button:hover {
    color: #ccc;
    border-color: #ccc;
  }
  .elem-item .toggle-btn {
    padding: 0.5rem;
    border: 2px solid #3c3c3c;
    margin-right: 1rem;
  }
  .elem-item .toggle-btn.selected {
    color: #5EC8A1;
    border-color: #5EC8A1;
  }
  .elem-item .edit-btn,
  .elem-item .delete-btn {
    border: none;
  }

  .elem-item .delete-btn:hover {
    color: rgb(173, 25, 25);
  }
  .action-btns {
    display: flex;
    flex-grow: 1;
    flex-direction: column;
    align-items: flex-start;
  }

  .edit-wrap {
    display: flex;
    flex-wrap: wrap;
  }

  .btn-wrap {
    display: flex;
    justify-content: center;
    align-items: center;
  }

</style>