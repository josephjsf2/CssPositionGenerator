<template>
  <base-card>
    <template v-slot:title>
      <h2>Background</h2>
    </template>
    <template v-slot:content>
      <div class="form">
        <input class="select" v-model="srcType" type="radio" id="upload" value="upload">
        <label class="select-label" for="upload">Upload</label>
        <input class="select" v-model="srcType" type="radio" id="url" value="url">
        <label class="select-label" for="url">From Url</label>
        <!-- upload -->
        <div v-if="srcType === 'upload'">
          <input class="upload-input" type="file" allow="image" @change="upload($event)">
        </div>
        <!-- url -->
        <template v-else-if="srcType === 'url'">
          <label class="txt-label" for="url">Url</label>
          <input class="txt-input" type="text" v-model="inputUrl" id="url">
        </template>
        <div class="input-grp half">
          <label class="txt-label" for="width">W:</label>
          <input class="txt-input" type="text" v-model="width" id="width">
        </div>
        <div class="input-grp half">
          <label class="txt-label" for="height">H:</label>
          <input class="txt-input" type="text" v-model="height" id="height">
        </div>
        <base-button :solid="true" :primary="true" @click="setSize()">Set Size</base-button>
      </div>
    </template>
  </base-card>
</template>

<script>
export default {
  inject: ['uploadUrl', 'setBgSize'],
  data() {
    return {
      srcType: null,
      width: null,
      height: null,
      uploadedUrl: null,
      inputUrl: null
    }
  },
  methods: {
    upload(event) {
      const file = event.target.files[0];
      this.uploadedUrl = URL.createObjectURL(file);
      this.uploadUrl(this.uploadedUrl);
    },
    setSize() {
      if (this.srcType === 'url') {
        this.uploadUrl(this.inputUrl);
      }
      this.setBgSize(this.width, this.height);
    }
    
  },

}

</script>

<style scoped>
  .form {
    display: flex;
    padding: 0;
    flex-wrap: wrap;
  }
  .select {
    display: none;
  }
  .select:checked + .select-label {
    border: 2px solid #5EC8A1;
    color: #5EC8A1;
  }
  .select-label {
    display: inline-block;
    padding: 0.1rem 0.5rem;
    height: 2rem;
    line-height: 1.7rem;
    vertical-align: middle;
    border-radius: 1rem;
    border: 2px solid #707070;
    color: #707070;
    font-size: 1rem;
    text-align: center;
    margin: 0.5rem 0;
    margin-right: 0.5rem;
    cursor: pointer;
    flex-basis: 110px;
  }
</style>
